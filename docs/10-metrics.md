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