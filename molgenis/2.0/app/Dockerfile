FROM tomcat:7.0-jre8-alpine

# --no-cache allows users to install packages with an index that is updated and used on-the-fly and not cached locally
# This avoids the need to use --update and remove /var/cache/apk/* when done installing packages.
RUN apk --no-cache add openssl \	
	&& wget -O molgenis-app-2.0.1.war http://search.maven.org/remotecontent?filepath=org/molgenis/molgenis-app/2.0.1/molgenis-app-2.0.1.war

RUN rm -r $CATALINA_HOME/webapps/ROOT \
    && rm -r $CATALINA_HOME/webapps/docs \
    && rm -r $CATALINA_HOME/webapps/examples \
    && mv molgenis-app-2.0.1.war $CATALINA_HOME/webapps/ROOT.war \
	&& echo 'CATALINA_OPTS="-Xmx2g -XX:+UseConcMarkSweepGC -XX:+CMSClassUnloadingEnabled -Dmolgenis.home=/opt/molgenis/"' > $CATALINA_HOME/bin/setenv.sh

RUN mkdir -p /opt/molgenis \
	&& if ! test -f /opt/molgenis/molgenis-server.properties; then echo -e "db_user=molgenis\ndb_password=molgenis\ndb_uri=jdbc\:postgresql\://db/molgenis?reWriteBatchedInserts\=true&autosave\=CONSERVATIVE\nadmin.password=admin" > /opt/molgenis/molgenis-server.properties; fi

VOLUME /opt/molgenis

CMD ["catalina.sh", "run"]