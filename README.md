# LABORATOIRE 3 - GOOGLE APP ENGINE

Dans les prédédents laboratoire, nous nous sommes initiés aux infrastructures
cloud de type Infrastructure as a Service (IaaS). Nous avions dû déployer une
application web en utilisant les services web d'Amazon (AWS). Pour ce
laboratoire, nous nous intéressons aux infrastructures cloud de type Platform
as a Service (PaaS) et plus particulièrement au service Google App Engine (GAE).
Nous allons, premièrement, déployer une application web simple. Ensuite, nous
allons écrire notre propre application web, utilisant des service GAE. Pour
terminer, nous testerons sa performance.

## ÉTUDIANTS

* FRANCHINI Fabien
* DONGMO NGOUMNAÏ Annie Sandra

## TABLE DES MATIÈRES

1. [Tâche 1: Déployment d'une application web simple](#t%C3%82che-1-deployment-dune-application-web-simple)
2. [Tâche 2: Développement d'un servlet utilisant le datastore](#t%C3%82che-2-developpement-dun-servet-utilisant-le-datastore)
3. [Tâche 3: Test de performance des écritures dans le datastore](#t%C3%82che-3-test-de-performance-des-%C3%89critures-dans-le-datastore)
4. [Conclusion](#conclusion)

## TÂCHE 1: DEPLOYMENT D'UNE APPLICATION WEB SIMPLE

Dans ce chapitre, nous allons déployer une application simpliste pour découvrir
le service GAE. Premièrement, nous allons initialiser un nouveau projet dans
l'IDE Eclipse. Ensuite, nous allons le déployer sur GAE.

Pour la suite, nous assumons qu'un compte GMail et GAE a été créé. De plus,
utilisons la machine virtuelle pré-configurée pour ce laboratoire. Pour de plus
amples informations, se référer au
[tutoriel de mise en route](https://cyberlearn.hes-so.ch/mod/page/view.php?id=665111).

![Eclipse initialization step-by-step](assets/images/01-eclipse_initialization_step_1.png)

![Eclipse initialization step-by-step](assets/images/01-eclipse_initialization_step_2a.png)

![Eclipse initialization step-by-step](assets/images/01-eclipse_initialization_step_2b.png)

![Eclipse initialization step-by-step](assets/images/01-eclipse_initialization_step_3.png)

![Eclipse initialization step-by-step](assets/images/01-eclipse_initialization_step_4a.png)

![Eclipse initialization step-by-step](assets/images/01-eclipse_initialization_step_4b.png)

![Eclipse initialization step-by-step](assets/images/01-eclipse_initialization_step_5.png)

![Eclipse initialization step-by-step](assets/images/01-eclipse_initialization_step_6.png)
A la suite de cette création nous avons une serie de fichiers et package généré automatiquement nous allons nous intéressé plus particulièrementau fichier Lab03Servelet.java, web.xml et appengine-web.xml.
Le code suivant est généré automatiquement dans le fichier Lab03Servelet on remarque qu'il s'agit là d'un servelet ayant pour but de répondre aux requêtes HTTP. Ainsi d'après ce code il devrait (le servelet) répondre à la requête HTTP de type GET au travers de la methode doGet en envoyant un message "hello world" en clair.

```java
package ch.heigvd.cld.lab;

import java.io.IOException;

@SuppressWarnings("serial")
public class Lab03Servlet extends HttpServlet {
    public void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    resp.setContentType("text/plain");
    resp.getWriter().println("Hello, world");
    }
}
```
Le code suivant est celui du fichier web.xml. En effet, il s'agit d'un fichier de configuration classique de notre Java EE ou plus exactement d'un fichier descripteur de deploiement. Ainsi, il décrit les classes et les configurations de notre application web on voit par exemple là que notre servelet Lab03 est appelé lorsque le visiteeur essaie d'accéder à la page /lab03 de notre site. On peut deduire de cela que ce fichier permet d'effectuer un mapping entre l'URL de la requête et un code worker.

```xml
<?xml version="1.0" encoding="utf-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://java.sun.com/xml/ns/javaee"
xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" version="2.5">
	<servlet>
		<servlet-name>Lab03</servlet-name>
		<servlet-class>ch.heigvd.cld.lab.Lab03Servlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>Lab03</servlet-name>
		<url-pattern>/lab03</url-pattern>
	</servlet-mapping>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
	</welcome-file-list>
</web-app>
```
Le code suivant est celui du fichier appengine-web.xml. Il s'agit là d'un fichier de configuration propre à google appengine, il permet de donner des informations sur notre application app Engine (nom, version et autre), d'activer certaine fonctionnalités comme les sessions, services googles et permet également à l'application de traiter ou pas les requêtes concurrentes par la balise ``threadsafe`` 

```xml
<?xml version="1.0" encoding="utf-8"?>
<appengine-web-app xmlns="http://appengine.google.com/ns/1.0">
  <application></application>
  <version>1</version>

  <!--
    Allows App Engine to send multiple requests to one instance in parallel:
  -->
  <threadsafe>true</threadsafe>

  <!-- Configure java.util.logging -->
  <system-properties>
    <property name="java.util.logging.config.file" value="WEB-INF/logging.properties"/>
  </system-properties>

  <!--
    HTTP Sessions are disabled by default. To enable HTTP sessions specify:

      <sessions-enabled>true</sessions-enabled>

    It's possible to reduce request latency by configuring your application to
    asynchronously write HTTP session data to the datastore:

      <async-session-persistence enabled="true" />

    With this feature enabled, there is a very small chance your app will see
    stale session data. For details, see
    https://cloud.google.com/appengine/docs/java/config/appconfig#Java_appengine_web_xml_Enabling_sessions
  -->

</appengine-web-app>
```

![Run application locally step-by-step](assets/images/01-run_locally_step_1.png)

![Run application locally step-by-step](assets/images/01-run_locally_step_2.png)

![Run application locally step-by-step](assets/images/01-run_locally_step_3.png)

![Run application locally step-by-step](assets/images/01-run_locally_step_4.png)

![Run application locally step-by-step](assets/images/01-run_locally_step_5.png)

![GAE create project](assets/images/01-gae_init_step_1.png)

![GAE create project](assets/images/01-gae_init_step_2.png)

![GAE create project](assets/images/01-gae_init_step_3.png)

![Deploy application on GAE step-by-step](assets/images/01-deploy_web_app_step_1.png)

![Deploy application on GAE step-by-step](assets/images/01-deploy_web_app_step_2.png)

![Deploy application on GAE step-by-step](assets/images/01-deploy_web_app_step_3.png)

![Deploy application on GAE step-by-step](assets/images/01-deploy_web_app_step_4.png)

![Deploy application on GAE step-by-step](assets/images/01-deploy_web_app_step_5.png)

## TÂCHE 2: DEVELOPPEMENT D'UN SERVET UTILISANT LE DATASTORE

## TÂCHE 3: TEST DE PERFORMANCE DES ÉCRITURES DANS LE DATASTORE

## CONCLUSION
