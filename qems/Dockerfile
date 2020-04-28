FROM tomcat:8.5.54-jdk8-openjdk

ARG WEBHOME
ARG APIHOME

ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

COPY QEMS_Admin.war QEMS_API.war jce-policy-8.tar.gz /tmp/

RUN rm -rf $WEBHOME/webapps/* && \
	cp -R $WEBHOME $APIHOME && \
	\
	sed -i -e "22 s/8005/8011/" -e "69 s/8080/8081/" \
		-e "71 s/8443/8441/" -e "116 s/8009/8101/" \
		-e "116 s/8443/8441/" $WEBHOME/conf/server.xml && \
	unzip /tmp/QEMS_Admin.war -d $WEBHOME/webapps/ROOT && \
		rm -f /tmp/QEMS_Admin.war && \
	cd $WEBHOME/webapps/ROOT/WEB-INF/classes/config/properties && \
	sed -i -e "/server\.mode/ s/LOCAL/WARN/" -e "s/false/true/" common.properties && \
	sed -i -e "/^#.*$/d" -e "s/\/\/.\+:.*$/\/\/qdb:3306\/qems/" conf.properties && \
	sed -i -e "1 s/DEBUG/WARN/" log4j.properties && \
	tar xvfz /tmp/jce-policy-8.tar.gz -C $JAVA_HOME/jre/lib/security && \
		rm -f /tmp/jce-policy-8.tar.gz && \
	\
	unzip /tmp/QEMS_API.war -d $APIHOME/webapps/qt-oms && \
		rm -f /tmp/QEMS_API.war && \
	cd $APIHOME/webapps/qt-oms/WEB-INF/services/qtService && \
	sed -i -e "/^#.*$/d" -e "s/\/\/.\+:.*$/\/\/qdb:3306\/qems/" jdbc-db-mysql.properties && \
	cd $APIHOME/webapps/qt-oms/WEB-INF/classes && \
	sed -i -e "5 s/INFO/WARN/" -e "5 s/CONSOLE/STDOUT/" log4j.properties

WORKDIR $WEBHOME
