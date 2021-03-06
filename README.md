# aplicationServer
Instalacion y configuracion de java, tomcat, maven, scm, jenkins, artifactory, cas, maven 

# descarga de los ficheros necesarios

* jdk-7
* apache-tomcat
* apache-maven
* artifactory
* jenkins
* scm
* cas

# Preparar Sistema Operativo (CentOs 7)
```
yum install nano
```

# Descomprimir Java:
```
cd /opt
tar zxvf jdk-7xxxx.gz
ln -s jdk-7xxxx java
```

# Descomprimir Tomcat:
```
cd /opt
tar xvf apache-tomcatxxx.tar.gz
ln -s apache-tomcat-xxxxx tomcat
```

# Descomprimir maven:
```
cd /opt
tar xvf apache-mavenxxxx.tar.gz
ln -s apache-mavenxxxxx maven
```

# Configurar variables de entorno
```
cd /etc/profile.d/
nano java-tomcat.sh
```
Copiar el siguiente trozo de texto
```
export JAVA_HOME=/opt/java
export CATALINA_HOME=/opt/tomcat
export JAVA_OPTS="-server -Xss```
256k -Xms256m -Xmx512m -XX:MaxPermSize=512m -Duser.timezone=Europe/Madrid-Dorg.slf4j.simpleLogger.defaultLogLevel=debug"
export M2_HOME=/opt/maven
export MAVEN_OPTS="-Xms256m -Xmx512m -XX:MaxPermSize=512m"export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/ro```
ot/bin:/opt/java/bin:/opt/maven/bin
```
Cargamos las propiedades...
```
source /etc/profile
```

# Copiar wars al tomcat:
```
cd /opt
Copiar el siguiente trozo de texto
cp scm-xxxxx.war tomcat/webapps/scm.war
cp jenkins.war tomcat/webapps/jenkins.war
cp artifactory.war tomcat/webapps/artifactory.war
```

# Eliminar aplicaciones no usadas:
```
cd /opt
rm -rf tomcat/webapps/ROOT
rm -rf tomcat/webapps/examples
rm -rf tomcat/webapps/docs
rm -rf tomcat/webapps/manager
rm -rf tomcat/webapps/host-manager
```

# Crear demonio tomcatd:
```
cd /etc/init.d/
nano tomcatd
```
Copiar el texto de abajo
```
# description: Tomcat Start Stop Restart  # processname: tomcat  
case $1 in  
   ;;   
  stop)     
     su -s /bin/bash - tomcat -c "\$CATALINA_HOME/bin/shutdown.sh "  

    ;;   
  restart)  
     su -s /bin/bash - tomcat -c "\$CATALINA_HOME/bin/shutdown.sh " 
esac      
exit 0
```
proesguir con estos comandos...
```
chmod 755 tomcatd
chkconfig --add tomcatd
useradd tomcat
passwd tomcat
```

# Establecer permisos tomcat:
```
cd /opt
chown -Rh root:tomcat tomcat 
cd tomcat
chwon -R tomcat:tomcat conf logs, work temp webapps
```

# Arrancar servidor tomcat:
```
service tomcatd start
```
