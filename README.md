Gbase.NLog.JsonTarget
=====================

A NLog target that posts Json to a http endpoint

Dependencies
------------

Currently using ServiceStack.Text for serialization and HttpClient to post asynchronously.

Usage
-----

Add Gbase.NLog.JsonTarget to your solution and update your nlog.config:

Add the assembly to the extensions section

    <extensions>
        <add assembly="Gbase.NLog.JsonTarget" />
    </extensions>

Add the target with a url & fields to post

    <targets async="true">
        <target name="SampleTarget" xsi:type="JsonPost"
                url="http://myrestservice.com/logjson/">

          <field name="time" layout="${date:universalTime=True:format=yyyy-MM-ddTHH\:mm\:ss.FFFZ}" />
          <field name="msg" layout="${message}" />
          <field name="src" layout="${logger}" />
          <field name="lvl" layout="${level}" />
          <field name="exception" />                <!--The actual exception object serialized to json-->
          <field name="properties" /> 
          <field name="keyprop" property="a key"/>  <!--A value in the properties bag under key 'a key'-->
          <field name="another" property="5"/>  
        </target>
    </targets>

*Field* can be a layout or property of the LogEventInfo class. In this example the exception and properties
of the log event will be sent as objects.

Add a rule to send log events to the target

    <rules>
        <logger name="*" minlevel="Debug" writeTo="SampleTarget"/>
    </rules>


Examples
--------

    var log = LogManager.GetLogger("myloggername");

    var le = new LogEventInfo();
    le.Level = LogLevel.Info;
    le.TimeStamp = DateTime.UtcNow;
    le.Message = "Hello, Json";
    le.Properties["a key"] = "a value";
    le.Properties[5] = 10;

    log.Log(le);

    *produces*

    { 
        "time" : "0001-01-01T05:00:00Z",
        "msg" : "Hello, Json",
        "lvl" : "Info",
        "src" : "myloggername",
        "properties" : { 
            "a key" : "a value",
            "5" : 10,
        },
        "keyprop" : "a key",
        "another" : 10
    }


Changes
-------

v1.0.0 Initial Release

v1.0.1 Added 'property' to log field

v1.0.2 Added 'logger' to field lookup (logger name)

License
--------

Released under MIT License. Enjoy!
