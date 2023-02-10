In order to utilize this application to upload sflow data to a rest api endpoint the agent must run on the host.  The api endpoint url will be the location of the listener API and the local SFLOW collector, for example SFLOW-RT or SFLOW Tools will need to be configured as such

sflow = receiver
udp = 6343
target = <host-running-rest-api>:8080


With $host-running-rest-api being the API endpoint that flow logs will be emitted to.
