# Operation Principles and Settings

## Functional Operation Scheme

Being an intermediate web service, the *Adapter* serves both as a client concerning the external service (for example, concerning the Credit Reference Bureau – CRB), and the SOAP web service, with regard to Loginom workflow (or any other outside information user). Serving as a web service, the *Adapter* provides the WSDL description according to the SOAP standard. The WSDL description enables a clear definition of parameters of interaction to be provided with the web service, by means of which such interaction is performed including the XML message formats. At the same time, serving as a client, the *Adapter* provides transformation of these messages from the SOAP format to the individual formats of the external services, and backwards.

The functional operation scheme of the *Adapter* is shown in Figure 7.

![Figure 7. The Functional Operation Scheme of the Adapter](./images/functional_diagram.svg)

## Configuration of the Client Role of the External Services

Algorithm of actions at the stage of the request sending to the external service is shown in Figure 8.

![Figure 8. The algorithm of Actions at the Stage of the Request Sending to the External Service](./images/algorithm_sending_request.svg)

As is shown in Figure 8, the first action after the SOAP request receipt from Loginom workflow is the XML request text selection from the SOAP package. According to the SOAP protocol description, the XML request to the web service is in the node body `Body` of the SOAP package. The SOAP package example is provided below:

```XML
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
xmlns:ded="http://www.basegroup.ru/DeductorIntegrationServer">
<soapenv:Header/>
    <soapenv:Body>
        <ded:Cheques>
            <variables>
                <DateMin>2000-06-06T23:59:00</DateMin>
                <DateMax>2017-06-06T23:59:00</DateMax>
            </variables>
        </ded:Cheques>
    </soapenv:Body>
</soapenv:Envelope>
```

The `ded:Cheques` node will be selected for the provided example from the SOAP package, and further transformation in the following stages will be performed with the XML text:

```XML
<ded:Cheques>
    <variables>
        <DateMin>2000-06-06T23:59:00</DateMin>
        <DateMax>2017-06-06T23:59:00</DateMax>
    </variables>
</ded:Cheques>
```

The first stage of the XML request transformation can be the XSLT transformation. This stage is configured by means of parameters of the XSLT *Configurator* *for transformation of the incoming message before transfer* and *XSLT Parameters*, as shown in Figure 9.

![Figure 9. Parameters of the XSLT Request Transformation Stage](./images/XSLT_transformation_phase.png)

The *XSLT for transformation of the incoming message before transfer* parameter contains the path to the XSLT script. The input parameters can be specified in the script as such, as it is represented by means of the following example:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:fo="http://www.w3.org/1999/XSL/Format">
<xsl:output method="xml" version="1.0" encoding="UTF-8" indent="yes"/>

<xsl:param name="MemberCode"/>
<xsl:param name="UserID"/>
<xsl:param name="Password"/>

<xsl:template match="@* | node()">
    <xsl:copy>
        <xsl:apply-templates select="@* | node()"/>
    . . . .
</xsl:stylesheet>
```

`MemberCode`, `UserID`, `Password` parameters, taking the values specified in the *Configurator*, are described in the script body.

If the value of the *>XSLT for transformation of the incoming message before transfer* parameter is not specified, the XSLT transformation stage is not performed.

The next stage can be transformation to the special message format of the credit reference bureau "UNITED CREDIT BUREAU". This transformation is specific for the work with the CRB specified above, and it is not applied for other tasks. This stage is configured by means of the *Special message format* parameter.

The next processing stage can be an application of the OS command. Performance of this stage can be defined by the following parameters: the *Configurator*: *The request file source name*, *The resulting name of the request file*, *The request file processing command *. The command example:

```
"C:\Program Files\7-Zip\7z" a -tzip "%outputFile%" "%inputFile%"
```

While command executing, `%inputFile%` and `%outputFile%` templates are replaced with the values of the following parameters: *The request file source name* and *The resulting name of the request file* correspondingly. Full paths to the files or only the files' names can be stated in these parameters. The files will be placed into the folder with `Web.config` in the case of full path absence.

Before the OS command execution  result of the previous transformation stages is represented as a file with the name specified in *The request file source name* parameter; then it is recorded to the disk. Then the OS command is applied to this file and the output file is formed. The output file content is then transferred to the next processing stage.

If the values of *The request file source name* and *The resulting name of the request file* parameters are not specified, the files names will be generated according to randomly generated GUID (Globally Unique Identifier), without extension.  These temporary files will be also placed into the folder with `Web.config`. They will be deleted after processing.

The files names can be also specified as a template with usage of the `%guid%` construction. For example, based on the `"%guid%_reg.xml"` template a randomly generated GUID will be inserted into the file name instead of `%guid%`.

These stage execution rules are also applied in the case of transformation of the service responses by the OS command with an application of the following parameters: *The response file source name*, *The resulting name of the response file*, *The response file processing command*. If *The request file processing command* parameter is not specified, then the *OS command* processing stage is not executed. The final result of the XML request transformation stages is the formation of the request in the individual format specified in the external service description, and it can be XML, JSON, binary data, text, etc. At the next stage, the request received in the result of the previous transformations in the case of the REST request formation is placed into the HTTP/HTTPS package body (*Entity Body*) sent to the external service. In the case of the SOAP request, it is placed into the  `Body` node of the SOAP package.

Type of sent request (REST or SOAP) is determined by the *Message format* parameter. The value of the `plainXml` parameter is set according to the REST message format. The value of the `soap` and `soap12` parameters are set according to the SOAP message format in Specifications 1.1 and 1.2 correspondingly.
While filling in the HTTP/HTTPS package sections sent to the external service, the following parameters of the *Configurator* are also used:

| Sections of the HTTP/HTTPS Package | The parameters of the *Configurator* used for filling in |
|:--------- |:-------------|
| Request line (*Request Line*) | *URL for request sending* |
| Headers (*Message Headers*) | In the `Content-Type` header, the value of the *ContentType for transfer in the header* parameter is stated. In the `SOAPAction` header, it is required to state the value of the *SOAPAction that is required to transfer in the header* parameter.  |
| Message body (*Entity Body*) | is filled in by the formed SOAP package or REST message body according to the value of the *Message format* parameter/  |

While interacting with the external services the *Adapter* uses only the POST requests. The GET requests are not used.

Example of the HTTP/HTTPS request package to the SOAP service:

*Request line (Request Line):*

```
POST https://stend-sup-29/DIS/Service.svc/ServiceClass HTTP/1.1
```

*Headers (Message Headers):*

```
Content-Type: text/xml; charset=UTF-8
SOAPAction: "http://www.basegroup.ru/WebServiceProxy/DIS_stend-sup-29/Cheques"
Content-Length: 499
Host: stend-057
```

*Message body (Entity Body):*

```XML
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
xmlns:ded="http://www.basegroup.ru/DeductorIntegrationServer">
<soapenv:Header/>
    <soapenv:Body>
        <ded:Cheques>
            <variables>
                <DateMin>2000-06-06T23:59:00</DateMin>
                <DateMax>2017-06-06T23:59:00</DateMax>
            </variables>
        </ded:Cheques>
    </soapenv:Body>
</soapenv:Envelope>
```

At the stage of the package sending during the HTTPS connection implementation, encryption with the cerificate usage is applied. To learn more about certificates installation and usage refer to the [Certificates Usage](./work-with-certificates.md) section.

The formed package is sent at the address specified in the *URL for the request sending* parameter.

The algorithm of actions at the stage of the response receipt from the external service is shown in

![Figure 10. The algorithm of Actions at the Stage of the Response Receipt from the External Service](./images/algorithm_receiving_response.svg)

In the case of a response receipt from the external service, the succession of the transformation stages is reverse. Upon receipt of the response message, it is decrypted (based on the HTTPS connection) and the response content is selected. In the case of the REST service, the response content is the message body (*Entity Body*) HTTP/HTTPS of the received package. In the case of the SOAP service, content of the `Body`node of the SOAP package is selected. Then the response content is transferred for processing by the OS command by applying the following parameters: *The response file source name*, *The resulting name of the response file*, *The response file processing command*. Then the operations reverse to the optional transformations and XSLT transformation with usage of the *XSLT for transformation of the response message*, and *XSLT parameters* parameters are performed. The final result of the transformation stages is the formation of the XML response, the structure of which complies with the XSD schema of the service description. This schema is used for the WSDL web service formation, and it is an obligatory component of the *Adapter* settings. The path to the schema is specified in *The schema to which the service description will refer the * parameter.

> **Important**: incompliance of the resulting XML response with its description in the XSD schema will cause the *Adapter* web service operation error.

There is a period when the *Adapter* waits for the response from the external service. The timeout period is expressed in seconds in the *Timeout* parameter. On the expiration of the timeout period, and if there is no response from the external service the exception is formed.

## Configuration of the SOAP Service Role

Apart from the sending and receipt of messages, the *Adapter* web service forms the WSDL service description. It is possible to get the WSDL description using the computer browser with the deployed *Adapter* at the following URL: `http://localhost/LoginomAdapter/Service.svc?wsdl`. When referring to the web service from the network, it is possible to use the following URL: `http://<ip address>/LoginomAdapter/Service.svc?wsdl` where `<ip address>` - is the computer IP address with the deployed *Adapter*.

The WSDL service is formed on the basis of the following parameters:

| The Group of the Parameters for the Web Service Role Configuration: |
|:--------- |
| The web service name |
| The web service description |
| The schema to which the service description will refer |
| Method name |
| Method name in the Loginom Adapter |
| Method description |
| The root element name of the SOAP response; Namespace of the root element of the SOAP response |
| Namespace of the root element of the SOAP response; Namespace of the root element of the SOAP response |

*The web service name*, *Method name*, *Method name in the Loginom Adapter* parameters participate in the formation of the elements' name of the service WSDL description.  For example, values of *The web service name* parameter form the unique names of the `<wsdl:portType>` elements describing the services represented by the *Adapter* web service, and values of the *Method name in the Loginom Adapter* parameter form the unique names of the `<wsdl:operation>` elements describing methods of these services.

The compulsory configuration component also participates in the WSDL formation process – the XSD schema of the service description; a path to it is specified in *The schema to which the service description will refer* the parameter. This schema enables the defining of the XML structure of responses and the requests of all methods of each service described in the `web.config` configuration file, and it is imported to WSDL in the `<wsdl:types>` section. Definitions of all requests and response messages are provided by the separate elements of the imported schema, and they are represented by the WSDL nodes `<wsdl:message>`.

Here is an example of the XSD schema import of the service description in the `<wsdl:types>` WSDL section and descriptions of the message formats based on the imported schema in the `<wsdl:message>` nodes:

```XML
<wsdl:definitions .... >
    <wsdl:types>
        <xsd:schema targetNamespace="http://www.basegroup.ru/WebServiceProxy/Imports">
            <xsd:import namespace="http://www.basegroup.ru/DeductorIntegrationServer" schemaLocation="/WebServiceProxy/Service.svc?xsd=xsd1"/>
        </xsd:schema>
    </wsdl:types>
    <wsdl:message name="DIS_stend-sup-29_Cheques_InputMessage">
        <wsdl:part name="parameters" element="q1:Cheques" xmlns:q1="http://www.basegroup.ru/DeductorIntegrationServer"/>
    </wsdl:message>
    <wsdl:message name="DIS_stend-sup-29_Cheques_OutputMessage">
        <wsdl:part name="parameters" element="q2:ChequesResponse" xmlns:q2="http://www.basegroup.ru/DeductorIntegrationServer"/>
    </wsdl:message>
    ....
</wsdl:definitions>
```

According to this example by the following reference: `/WebServiceProxy/Service.svc?xsd=xsd1`  `<xsd:import … >`, construction enables the importing of the XSD schema of the service description. (*tipe*) structure of the incoming message (request) of the `Cheques` method of the `DIS_stend-sup-29` service in the `<wsdl:message name="DIS_stend-sup-29_Cheques_InputMessage">` node is set by the `Cheques` element defined in this schema.  Similarly, to describe the incoming message (response) structure of this method, the `ChequesResponse` schema element is used.

Thus, the XSD schema of the service description must contain these elements and set their (*tipe*) structure; and the *Adapter* settings must be clear enough to understand what message is described by the particular element of the imported schema.  For this purpose, it is required to set a name and namespace of the corresponding schema element for the request and response of each method. These attributes clearly identify the elements in the XSD schema, and they are set by the following parameters:

* *The root element name of the SOAP response*;
* *Namespace of the root element of the SOAP response*;
* *The root element name of the SOAP request*;
* *Namespace of the root element of the SOAP request*.

Values of *The web service description* parameter are set by content of the `<wsdl:documentation>` WSDL nodes, and they are used for the arbitrary description of the services.
