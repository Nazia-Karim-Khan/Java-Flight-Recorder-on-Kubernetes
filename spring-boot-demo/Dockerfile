# FROM openjdk:17-jdk-slim

# WORKDIR /app

# # Copy application JAR and Cryostat agent JAR
# COPY target/spring-boot-demo-0.0.1-SNAPSHOT.jar app.jar
# COPY target/dependency/cryostat-agent-shaded.jar cryostat-agent-shaded.jar

# # Expose necessary ports: app, JMX, Cryostat agent callback
# EXPOSE 8080 9091 9977

# # JVM options: Attach Cryostat agent and enable JMX on port 9091
# ENV JAVA_TOOL_OPTIONS="-javaagent:/app/cryostat-agent-shaded.jar \
#  -XX:+FlightRecorder \
#  -XX:StartFlightRecording=duration=5m,filename=/tmp/recording.jfr,settings=profile \
#  -Dcom.sun.management.jmxremote \
#  -Dcom.sun.management.jmxremote.port=9091 \
#  -Dcom.sun.management.jmxremote.ssl=false \
#  -Dcom.sun.management.jmxremote.authenticate=false"

# ENTRYPOINT ["java", "-jar", "app.jar"]


FROM openjdk:17-jdk-slim

WORKDIR /app

COPY target/spring-boot-demo-0.0.1-SNAPSHOT.jar app.jar
COPY target/dependency/cryostat-agent-shaded.jar cryostat-agent-shaded.jar

EXPOSE 8080 9091 9977

# Set JAVA_TOOL_OPTIONS for HTTP (no TLS) communication with Cryostat
ENV JAVA_TOOL_OPTIONS="-javaagent:cryostat-agent-shaded.jar \
-Dcryostat.agent.baseuri=http://cryostat.cryostat.svc.cluster.local:8181 \
-Dcryostat.agent.webclient.tls.required=false \
-Dcryostat.agent.app.name=spring-boot-demo \
-Dcom.sun.management.jmxremote \
-Dcom.sun.management.jmxremote.port=9091 \
-Dcom.sun.management.jmxremote.ssl=false \
-Dcom.sun.management.jmxremote.authenticate=false"

CMD ["sh", "-c", "java $JAVA_TOOL_OPTIONS -jar app.jar"]
