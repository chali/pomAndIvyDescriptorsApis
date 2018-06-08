# Example project showing failure of publishing due invalid ivy.xml

Execute `./gradlew clean publish --stacktrace`.

This line `infoNode.appendNode('description', [:], project.description ?: '')` causes that ivy file is being validated during publications.

This line `extraInfo('customNamespace', 'myElement', 'value')` add element to a place which is not corect based on ivy xml schema here: http://ant.apache.org/ivy/history/2.3.0/ivyfile/info.html

Example of an error:

    Caused by: java.text.ParseException: xml parsing: ivy.xml:5:18: cvc-complex-type.2.4.a: Invalid content was found starting with element 'description'. One of '{WC[##other:""]}' is expected.

Example of an ivy incorrect ivy file:

    <?xml version="1.0" encoding="UTF-8"?>
    <ivy-module version="2.0">
      <info organisation="test" module="test" revision="1.0" status="integration" publication="20180608155350">
        <ns:myElement xmlns:ns="customNamespace">value</ns:myElement>
        <description></description>
      </info>
      <configurations>
        <conf name="compile" visibility="public"/>
        <conf name="default" visibility="public" extends="compile,runtime"/>
        <conf name="runtime" visibility="public"/>
      </configurations>
      <publications>
        <artifact name="test" type="jar" ext="jar" conf="compile"/>
      </publications>
      <dependencies/>
    </ivy-module>
