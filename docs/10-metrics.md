There are a couple of important points to understand before getting started:

When using the Artemis Prometheus Metrics Plugin neither the broker nor the applications "send" metrics to Prometheus. Prometheus itself must retrieve or "scrape" metrics from the broker. This is why the plugin comes with a servlet. The servlet exposes an HTTP endpoint that Prometheus can use to scrape metrics.
The Artemis Prometheus Metrics Plugin is part of the broker infrastructure. It is not to be deployed as part of an application. The plugin's jar and war files are deployed on the broker and configured in broker.xml and bootstrap.xml respectively.
The Artemis Prometheus Metrics Plugin provides integration with Prometheus using two modules:

artemis-prometheus-metrics-plugin: This provides the actual implementation of org.apache.activemq.artemis.core.server.metrics.ActiveMQMetricsPlugin and packages it with the Micrometer and Prometheus dependencies in an "uber" jar.

artemis-prometheus-metrics-plugin-servlet: This provides a war file containing a simple servlet which can be deployed to the broker's embedded web server which then Prometheus can use to scrape metrics.

Once you clone the Artemis Prometheus Metrics Plugin repository simply run mvn install to build these two modules. The output will be in their respective target directories.

After building the modules follow these steps to deploy and configure the Artemis Prometheus Metrics Plugin. If you have some kind of dev-ops group which manages and configures your broker then they would follow these steps.

Copy artemis-prometheus-metrics-plugin/target/artemis-prometheus-metrics-plugin-<VERSION>.jar to <ARTEMIS_INSTANCE>/lib.

Add this to your <ARTEMIS_INSTANCE>/etc/broker.xml:

<metrics-plugin class-name="org.apache.activemq.artemis.core.server.metrics.plugins.ArtemisPrometheusMetricsPlugin"/>
Create the directory <ARTEMIS_INSTANCE>/web.

Copy artemis-prometheus-metrics-plugin-servlet/target/metrics.war to <ARTEMIS_INSTANCE>/web.

Add this to the web element in <ARTEMIS_INSTANCE>/etc/bootstrap.xml:

<app url="metrics" war="metrics.war"/>


```
<metrics>
   <jvm-memory>true</jvm-memory> <!-- defaults to true -->
   <jvm-gc>true</jvm-gc> <!-- defaults to false -->
   <jvm-threads>true</jvm-threads> <!-- defaults to false -->
   <netty-pool>true</netty-pool> <!-- defaults to false -->
   <plugin class-name="org.apache.activemq.artemis.core.server.metrics.plugins.LoggingMetricsPlugin"/>
   <plugin class-name="su.zubarev.MyMetricsPlugin">
      <property key="host" value="artemis1.test" />
      <property key="port" value="5162" />
      <property key="foo" value="10" />
   </plugin>

</metrics>
```
