# aplicationServer
Instalacion y configuracion de java, tomcat, maven, scm, jenkins, artifactory, cas, maven 

descarga de los ficheros necesarios

* jdk-7
* apache-tomcat
* apache-maven
* artifactory
* jenkins
* scm
* cas

Preparar Sistema Operativo (CentOs 7)
yum install nano

Descomprimir Java:
cd /opt
tar zxvf jdk-7xxxx.gz
ln -s jdk-7xxxx java

Descomprimir Tomcat:
cd /opt
tar xvf apache-tomcatxxx.tar.gz
ln -s apache-tomcat-xxxxx tomcat

Descomprimir maven:
cd /opt
tar xvf apache-mavenxxxx.tar.gz
ln -s apache-mavenxxxxx maven

Configurar variables de entorno
cd /etc/profile.d/
nano java-tomcat.sh
--------
export JAVA_HOME=/opt/java
export CATALINA_HOME=/opt/tomcat
export JAVA_OPTS="-server -Xss256k -Xms256m -Xmx512m -XX:MaxPermSize=512m -Duser.timezone=Europe/Madrid -Dorg.slf4j.simpleLogger.defaultLogLevel=debug"
export M2_HOME=/opt/maven
export MAVEN_OPTS="-Xms256m -Xmx512m -XX:MaxPermSize=512m"
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/opt/java/bin:/opt/maven/bin
--------
source /etc/profile

Copiar wars al tomcat:
cd /opt
cp scm-xxxxx.war tomcat/webapps/scm.war
cp jenkins.war tomcat/webapps/jenkins.war
cp artifactory.war tomcat/webapps/artifactory.war

Eliminar aplicaciones no usadas:
cd /opt
rm -rf tomcat/webapps/ROOT
rm -rf tomcat/webapps/examples
rm -rf tomcat/webapps/docs
rm -rf tomcat/webapps/manager
rm -rf tomcat/webapps/host-manager

Crear demonio tomcatd:
cd /etc/init.d/
nano tomcatd
----------------------
#!/bin/bash  
# description: Tomcat Start Stop Restart  
# processname: tomcat  
# chkconfig: 234 20 80  
#export CATALINA_HOME=/opt/apache-tomcat-7.0.35
#export JAVA_OPTS="-server -Xms256m -Xmx512m -XX:MaxPermSize=512m -XX:+UseConcMarkSweepGC -XX:+CMSPermGenSweepingEnabled -XX:+CMSClassUnloadingEnabled -Dorg.slf4j.simpleLogger.defaultLogLevel=debug"
 
case $1 in  
  start)  
     su -s /bin/bash - tomcat -c "\$CATALINA_HOME/bin/startup.sh " 
   ;;   
  stop)     
     su -s /bin/bash - tomcat -c "\$CATALINA_HOME/bin/shutdown.sh "  
    ;;   
  restart)  
     su -s /bin/bash - tomcat -c "\$CATALINA_HOME/bin/shutdown.sh " 
     su -s /bin/bash - tomcat -c "\$CATALINA_HOME/bin/startup.sh "
    ;;   
esac      
exit 0
----------------------
chmod 755 tomcatd
chkconfig --add tomcatd
useradd tomcat
passwd tomcat

Establecer permisos tomcat:
cd /opt
chown -Rh root:tomcat tomcat
cd tomcat
chown -R tomcat:tomcat logs temp webapps work

Arrancar servidor tomcat:
service tomcatd start






