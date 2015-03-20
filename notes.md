Presentation-tier webscripts
- Location: ./tomcat/shared/classes/alfresco
	/web-extension
		Share constom configuration(share-config-custom.xml).
		./site-data: contain Surf configuration XML files, such as page definitions, template-instances and components.
		./site-webscripts:  contain your presentation tier web scripts(*.desc.xml, *.js, *.ftl), consisting of description files, JavaScript controllers and FreeMarker template files.

In Alfresco, what is coded in Java and what is coded in webscripts(using Javascript and FreeMarker, but not jsp)?
What is the consideration? 

Java annotation programming. in Spring Framework.
FreeMarker language VS Markdown Language

Base on Spring framework, the Controllers, Views and Models are implemented by Java Beans.
- Controllers
- Views
- Models
- Dispatcher
	Implemented by a dispacher servlet in Spring MVC application which handles the request, executes the MVC pattern, and tries to identify a controller to handle the incoming request.

Surf webscript engine: to execute a webscript and render the content.
How the data to render in the FreeMarker file is accessed?
- what variables are used.

Surf objects are defined by XML files. There are 6 common types of Surf objects that are used in Alfresco application.
The classpath for those objects are:
- /alfresco/site-data
	contains core Alfresco objects
- /alfresco/web-extension/site-data
	contains the thirdparty overrides/extensions (files with same name will be overriden)
Files in web-extensions/site-data are processed after the ones in site-data directpory.
The 6 Surf objects:
1. component
	Ref: http://docs.alfresco.com/5.0/references/surf-object-xml-reference-component.html
	A component is a binding of a page-region with a Surf rendering engine(Here is a web script) who is responsible for generating the compenent's markup.
	The targetting webscript is indicated by the 'url' element of this object. 
	When an url is indicated in the definition file of this component, then Surf framework will find the indicated webscript by invoking the url which in turn
	let the webscript do the related work.
	The indicated webscript is found in the alfresco/site-webscript directory (compenents subdirectory corresponded). 
	Example:
		for the footer component, it is defined in the file alfresco/site-data/components/global.footer.xml
			<?xml version='1.0' encoding='UTF-8'?>
			<component>
			   <scope>global</scope>
			   <region-id>footer</region-id>
			   <source-id>global</source-id>
			   <url>/components/footer</url>
			</component>
		The the webscript corresponding to the url /components/footer locates in alfresco/site-webscripts/components/footer directory and it is 
		invoked by sending a HTTP method(GET etc.). Then a controller js script is executed and a FreeMarker markup file is rendered and the returned as the 
		HTTP response, which is the markup for that region.
	That webscript is describled by the file footer.get.desc.xml. The goal of desc.xml file is to bind the HTTP request with a specified javascript and the corresponding 
	FreeMarker file as the HTTP response.
	So the Surf Framework should be able to dispache the URIs to its corresponding handling webscript. The implementation maybe through the parsing of the URI and generating
	a directory accordind to certain conventions. Such as:
	for URI /components/footer , it will search in directory alfresco/site-webscripts/org/alfresco/components/footer.
	No need to retrive all desc.xml file and make an index. Just need a clear and well-organized directory structure.

	So how the disapcher is implemented? Through java classes?
2. Configuration

3. Page

4. Template instance

5. Template type
	A template type can have many template instances of that type. 
	It defines one or more rendering processors. It may define the common properties that all the template instances 
	of that given template type will receive to process at render time.
6. Theme
All components are defined in the directory: alfresco/site-data


Alfresco Content application server
Web scripts can interact with java code.
Users of a web script only interact through the web script interface, which comprises its URI, HTTP method, and request/response document types.
- Reporistory level (Data level) web scripts
	Executed to do data processing(query, retrive, update, delete).
	Typical request and response format is XML and Json 

- Presentation level web scripts
	Can be hosted in the Alfresco Content Application server or in a seperate presentation server.
	If seperated, it interact with the data webscripts through the Repository REST API. 

--  What comprise a web script?
A web script is composed of three parts:
- A descriptor file(required, only one)
	XML file. Root element is <webscript></webscript>
	A full list of elements allowed see link: http://docs.alfresco.com/5.0/references/api-wsdl-webscript-descriptor-language-reference.html

- A controller file(optional)
	Do repository data processing. 
	Can access the URI query string, Alfresco content application services, content reporistory data entry  
	-- query: to generate models(a set of data)
	-- update: POST, PUT, DELETE
- A response template(required, one or more)


Surf lets you bind components to regions. A component usually associates a region with a web script.
A surf template is a combination use of several web scripts into an overrall markup structure. 
Alfresco Share is an example of a Surf application whose pages are constructed through reuse of templates.
Three scopes: 
- global
	rendered only once 
- template
	rendered in each type of template. 
- page
	rendered in each page

Surf content
- 

Dashlets and Pages
Page URI and webscript URI

Créer un projet AMP sous Eclipse 
- 

-- Document pour le client
- 	Les évolutions démandées par le client sont déjà réalisées sur Alfresco Enterprise Edition 4.1.4 
	par une manière naive qui n’ont pas respecté les préconisations Alfresco, c’est-à-dire de packager 
	les développements en AMP. Je vais préciser le procédure en général à integrer une développement 
	spécifique vers un AMP et en déployer sur Alfresco Enterprise Edition 5.

- 	Pour l'ensemble des évolutions, je précise l'objectif de chaque évolution et les codes à réprendre 
	pour la réalisation(les fichiers à mettre à jour, supprimés ou ajoutés). 

-  Travaux restants à réaliser selon la propostion de migration par M. Buyukkalender.

Est ce que j'ajoute ma rédaction directment dans la doc de M. Buyukkalender? 

-- Document interne
- Un guide du développement de l'Alfresco Module Package sous Ecplise avec Maven. 

class path resource: is the resource of classes and JAR file that are visible to this application

Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'Authentication' defined in class path resource [alfresco/authentication-services-context.xml]: Invocation of init method failed; nested exception is java.lang.IllegalStateException: Invalid type cerbere specified for Authentication subsystem. No context file found
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1513)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:521)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:458)
	at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:293)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:223)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:290)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:191)
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveReference(BeanDefinitionValueResolver.java:328)
	... 104 more
Caused by: java.lang.IllegalStateException: Invalid type cerbere specified for Authentication subsystem. No context file found
	at org.alfresco.repo.management.subsystems.ChildApplicationContextFactory.afterPropertiesSet(ChildApplicationContextFactory.java:285)
	at org.alfresco.repo.management.subsystems.ChildApplicationContextFactory.<init>(ChildApplicationContextFactory.java:201)
	at org.alfresco.repo.management.subsystems.DefaultChildApplicationContextManager$ApplicationContextManagerState.updateOrder(DefaultChildApplicationContextManager.java:440)
	at org.alfresco.repo.management.subsystems.DefaultChildApplicationContextManager$ApplicationContextManagerState.<init>(DefaultChildApplicationContextManager.java:237)
	at org.alfresco.repo.management.subsystems.DefaultChildApplicationContextManager.createInitialState(DefaultChildApplicationContextManager.java:168)
	at org.alfresco.repo.management.subsystems.AbstractPropertyBackedBean.doInit(AbstractPropertyBackedBean.java:372)
	at org.alfresco.repo.management.subsystems.AbstractPropertyBackedBean.init(AbstractPropertyBackedBean.java:346)
	at org.alfresco.repo.management.subsystems.AbstractPropertyBackedBean.afterPropertiesSet(AbstractPropertyBackedBean.java:335)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1572)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1510)
	... 111 more

- Analyse
	When the bean 'Authentication' is being initialized througth the method org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean
	the indicated Authentication type 'cerbere' is not recognized because the context file about the cerbere subsystem is not loaded by Spring framework. 
	The type cerbere is configured in the file alfresco-global.properties in the <tomcat_home>/shared/classes/alfresco/ repository.
	authentication.chain=cerbere1:cerbere
- Solution
	Remove authentication.chain=cerbere1:cerbere from the alfresco-global.properties file.

File Override and recovery in AMP
For 

##############################################################################
Alfresco Share customization and more advanced development
##############################################################################
Ways to do customization on Share:
- Through configuring the configuration files
The main aspects of Share that can be configured are:

    General properties (for example port number, username and password)
    UI Controls
    Forms (custom types, built-in types, search, workflow)
    Menu bar
    Themes
    Custom types and aspects

    

##############################################################################
Alfresco Usefull sites
##############################################################################
http://ohej.dk/category/alfresco/
http://docs.alfresco.com/5.0/

##############################################################################
Alfresco User roles and permissions
##############################################################################
A user's role determines what they can and cannot do in a site. Each role has a default set of permissions.
The following sections describe these permissions. In general:

    - Managers have full rights to all site content - what they have created themselves and  what other site members have created.
    - Collaborators have full rights to the site content that they own; they have rights to  edit but not delete content created by other site members.
    - Contributors have full rights to the site content that they own; they cannot edit or  delete content created by other site members.
    - Consumers have view-only rights in a site: they cannot create their own content.

Site content can be defined as any content created or added to a site. This includes, but is not limited to, wiki pages, blog postings, library folders and items, calendar events, discussion topics, and comments on any content.


##############################################################################
Alfresco User Quota
##############################################################################
 In the Quota box, specify the maximum space available for this user and select the appropriate unit (GB, MB, or KB).

This information is not required. When no quota is provided, the user has no space limitations.

Content quotas are disabled by default. You can change the default setting by adding the following property to the alfresco-global.properties file: system.usages.enabled=true. 

----------------------------
User quota in database level is presented by the column qname_id with value of 127 in alf_node_properties table., and the node_id can be the user. So each user is represented by an ID (node_id)in the repository database.

It's possible that user's current content usage is greater than its quota only when the administrator set the quota explicitly when editing the user's profile, or the developer has changed the user's quota value in the database level. This won't cause the execeptions.

##############################################################################
- Alfresco 4: Alfresco Web client configuration
- https://wiki.alfresco.com/wiki/Web_Client_Configuration_Guide
##############################################################################
- Location of Extension Configuration Files:
Alfresco looks for a file called web-client-config-custom.xml on the classpath in the alfresco.extension package.
	JBoss: <alfresco>/jboss/server/default/conf/alfresco/extension
	Tomcat: <alfresco>/tomcat/shared/classes/alfresco/extension


##############################################################################
Alfresco backup
##############################################################################
The following locations should be attentioned:
- <alfresco_home>/alf_data
- <tomcat_home>/shared/classes

##############################################################################
Alfresco Feagures control
##############################################################################
Reference: http://docs.alfresco.com/5.0/concepts/maincomponents-disable.html

##############################################################################
Alfresco database schema
##############################################################################
Reference: https://svn.alfresco.com/repos/alfresco-open-mirror/alfresco/HEAD/root/projects/repository/config/alfresco/dbscripts/db-schema-context.xml

Files descriptions:
When h2-support using the scripts it provides to create the alfresco database in h2 database:
1. 	when connecting to h2 database using:
		INFO  [alfresco.repo.admin] [localhost-startStop-1] Using database URL 'jdbc:h2:./alf_data_dev/h2_data/alf_dev;MODE=PostgreSQL;AUTO_SERVER=TRUE;DB_CLOSE_ON_EXIT=FALSE;LOCK_TIMEOUT=10000;MVCC=FALSE;LOCK_MODE=0' with user 'alfresco'.
	If the database alfresco doesn't exist yet, then h2 will create that for. 
	ATTENTION to H2 database settings:
		databaseToUpper
			Database setting DATABASE_TO_UPPER (default: true).
			Database short names are converted to uppercase for the DATABASE() function, and in the CATALOG column of all database meta data methods. Setting this to "false" is experimental. When set to false, all identifier names (table names, column names) are case sensitive (except aggregate, built-in functions, data types, and keywords).
	The table names written in the scripts are all in lower case, but then be transfered to upper case when being created in h2. In a result, in the database schema generated(.xml files) the objects are all in upper case. 
	But in the scheme of the comparing source, all the objects are in lower case!!! That's why the compare validator say that 
scripts from H2 support | postgresql scripts officially released | generated from h2 support

Transferring:
	Data types 
	primary key name structure
	table name and column name case
When generating database schema, the table are succussfully generated, but the sequences are failed. Why? But they are existed exactly.

## ERRORS ##
AlfrescoCreate-ContentUrlEncryptionTables.sql
- des
Syntax error in SQL statement "INSERT INTO alf_applied_patch
  (id, description, fixes_from_schema, fixes_to_schema, applied_to_schema, target_schema, applied_on_date, applied_to_server, was_executed, succeeded, report)
  VALUES
  (
    'patch.db-V5.0-ContentUrlEncryptionTables', 'Manually executed script upgrade V5.0: Content Url Encryption Tables',
    0, 8001, -1, 8002, null, 'UNKNOWN', ${TRUE}, ${TRUE}, 'Script completed'
  [*])" [42000-186] 42000/42000 (Aide)

- cause
${TRUE}, ${TRUE} is not supported by h2

AlfrescoCreate-UsageTables.sql
- des
Syntax error in SQL statement "INSERT INTO alf_applied_patch
  (id, description, fixes_from_schema, fixes_to_schema, applied_to_schema, target_schema, applied_on_date, applied_to_server, was_executed, succeeded, report)
  VALUES
  (
    'patch.db-V3.4-UsageTables', 'Manually executed script upgrade V3.4: Usage Tables',
    0, 113, -1, 114, null, 'UNKNOWN', ${TRUE}, ${TRUE}, 'Script completed'
  [*])" [42000-186] 42000/42000 (Aide)

- cause
${TRUE}, ${TRUE} is not supported by h2

AlfrescoCreate-TenantTables.sql
- des
Syntax error in SQL statement "INSERT INTO alf_applied_patch
  (id, description, fixes_from_schema, fixes_to_schema, applied_to_schema, target_schema, applied_on_date, applied_to_server, was_executed, succeeded, report)
  VALUES
  (
    'patch.db-V4.0-TenantTables', 'Manually executed script upgrade V4.0: Tenant Tables',
    0, 6004, -1, 6005, null, 'UNKNOWN', ${TRUE}, ${TRUE}, 'Script completed'
  [*])" [42000-186] 42000/42000 (Aide)

- cause
${TRUE}, ${TRUE} is not supported by h2

AlfrescoPostCreate-JBPM-FK-indexes.sql
-- 
-- Title:      Upgrade to V3.4 - Add indexes for jbpm foreign keys
-- Database:   Generic
-- Since:      V3.4 schema 4204
-- Author:     pavelyur
-- 
-- Please contact support@alfresco.com if you need assistance with the upgrade.
-- 

CREATE INDEX FK_ACTION_REFACT ON JBPM_ACTION (REFERENCEDACTION_);
Table "JBPM_ACTION" not found; SQL statement:
-- 
-- Title:      Upgrade to V3.4 - Add indexes for jbpm foreign keys
-- Database:   Generic
-- Since:      V3.4 schema 4204
-- Author:     pavelyur
-- 
-- Please contact support@alfresco.com if you need assistance with the upgrade.
-- 

CREATE INDEX FK_ACTION_REFACT ON JBPM_ACTION (REFERENCEDACTION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_CRTETIMERACT_TA ON JBPM_ACTION (TIMERACTION_);
Table "JBPM_ACTION" not found; SQL statement:
--(optional)
CREATE INDEX FK_CRTETIMERACT_TA ON JBPM_ACTION (TIMERACTION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_ACTION_PROCDEF ON JBPM_ACTION (PROCESSDEFINITION_);
Table "JBPM_ACTION" not found; SQL statement:
--(optional)
CREATE INDEX FK_ACTION_PROCDEF ON JBPM_ACTION (PROCESSDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_ACTION_EVENT ON JBPM_ACTION (EVENT_);
Table "JBPM_ACTION" not found; SQL statement:
--(optional)
CREATE INDEX FK_ACTION_EVENT ON JBPM_ACTION (EVENT_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_ACTION_ACTNDEL ON JBPM_ACTION (ACTIONDELEGATION_);
Table "JBPM_ACTION" not found; SQL statement:
--(optional)
CREATE INDEX FK_ACTION_ACTNDEL ON JBPM_ACTION (ACTIONDELEGATION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_ACTION_EXPTHDL ON  JBPM_ACTION(EXCEPTIONHANDLER_);
Table "JBPM_ACTION" not found; SQL statement:
--(optional)
CREATE INDEX FK_ACTION_EXPTHDL ON  JBPM_ACTION(EXCEPTIONHANDLER_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_BYTEARR_FILDEF ON JBPM_BYTEARRAY (FILEDEFINITION_);
Table "JBPM_BYTEARRAY" not found; SQL statement:
--(optional)
CREATE INDEX FK_BYTEARR_FILDEF ON JBPM_BYTEARRAY (FILEDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_BYTEBLOCK_FILE ON JBPM_BYTEBLOCK (PROCESSFILE_);
Table "JBPM_BYTEBLOCK" not found; SQL statement:
--(optional)
CREATE INDEX FK_BYTEBLOCK_FILE ON JBPM_BYTEBLOCK (PROCESSFILE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_COMMENT_TOKEN ON JBPM_COMMENT (TOKEN_);
Table "JBPM_COMMENT" not found; SQL statement:
--(optional)
CREATE INDEX FK_COMMENT_TOKEN ON JBPM_COMMENT (TOKEN_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_COMMENT_TSK ON JBPM_COMMENT (TASKINSTANCE_);
Table "JBPM_COMMENT" not found; SQL statement:
--(optional)
CREATE INDEX FK_COMMENT_TSK ON JBPM_COMMENT (TASKINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_DECCOND_DEC ON JBPM_DECISIONCONDITIONS (DECISION_);
Table "JBPM_DECISIONCONDITIONS" not found; SQL statement:
--(optional)
CREATE INDEX FK_DECCOND_DEC ON JBPM_DECISIONCONDITIONS (DECISION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_DELEGATION_PRCD ON JBPM_DELEGATION (PROCESSDEFINITION_);
Table "JBPM_DELEGATION" not found; SQL statement:
--(optional)
CREATE INDEX FK_DELEGATION_PRCD ON JBPM_DELEGATION (PROCESSDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_EVENT_PROCDEF ON JBPM_EVENT (PROCESSDEFINITION_);
Table "JBPM_EVENT" not found; SQL statement:
--(optional)
CREATE INDEX FK_EVENT_PROCDEF ON JBPM_EVENT (PROCESSDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_EVENT_TRANS ON JBPM_EVENT (TRANSITION_);
Table "JBPM_EVENT" not found; SQL statement:
--(optional)
CREATE INDEX FK_EVENT_TRANS ON JBPM_EVENT (TRANSITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_EVENT_NODE ON JBPM_EVENT (NODE_);
Table "JBPM_EVENT" not found; SQL statement:
--(optional)
CREATE INDEX FK_EVENT_NODE ON JBPM_EVENT (NODE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_EVENT_TASK ON JBPM_EVENT (TASK_);
Table "JBPM_EVENT" not found; SQL statement:
--(optional)
CREATE INDEX FK_EVENT_TASK ON JBPM_EVENT (TASK_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_JOB_PRINST ON JBPM_JOB (PROCESSINSTANCE_);
Table "JBPM_JOB" not found; SQL statement:
--(optional)
CREATE INDEX FK_JOB_PRINST ON JBPM_JOB (PROCESSINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_JOB_ACTION ON JBPM_JOB (ACTION_);
Table "JBPM_JOB" not found; SQL statement:
--(optional)
CREATE INDEX FK_JOB_ACTION ON JBPM_JOB (ACTION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_JOB_TOKEN ON JBPM_JOB (TOKEN_);
Table "JBPM_JOB" not found; SQL statement:
--(optional)
CREATE INDEX FK_JOB_TOKEN ON JBPM_JOB (TOKEN_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_JOB_NODE ON JBPM_JOB (NODE_);
Table "JBPM_JOB" not found; SQL statement:
--(optional)
CREATE INDEX FK_JOB_NODE ON JBPM_JOB (NODE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_JOB_TSKINST ON JBPM_JOB (TASKINSTANCE_);
Table "JBPM_JOB" not found; SQL statement:
--(optional)
CREATE INDEX FK_JOB_TSKINST ON JBPM_JOB (TASKINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_SOURCENODE ON JBPM_LOG (SOURCENODE_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_SOURCENODE ON JBPM_LOG (SOURCENODE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_DESTNODE ON JBPM_LOG (DESTINATIONNODE_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_DESTNODE ON JBPM_LOG (DESTINATIONNODE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_TOKEN ON JBPM_LOG (TOKEN_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_TOKEN ON JBPM_LOG (TOKEN_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_TRANSITION ON JBPM_LOG (TRANSITION_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_TRANSITION ON JBPM_LOG (TRANSITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_TASKINST ON JBPM_LOG (TASKINSTANCE_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_TASKINST ON JBPM_LOG (TASKINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_CHILDTOKEN ON JBPM_LOG (CHILD_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_CHILDTOKEN ON JBPM_LOG (CHILD_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_OLDBYTES ON JBPM_LOG (OLDBYTEARRAY_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_OLDBYTES ON JBPM_LOG (OLDBYTEARRAY_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_SWIMINST ON JBPM_LOG (SWIMLANEINSTANCE_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_SWIMINST ON JBPM_LOG (SWIMLANEINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_NEWBYTES ON JBPM_LOG (NEWBYTEARRAY_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_NEWBYTES ON JBPM_LOG (NEWBYTEARRAY_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_ACTION ON JBPM_LOG (ACTION_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_ACTION ON JBPM_LOG (ACTION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_VARINST ON JBPM_LOG (VARIABLEINSTANCE_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_VARINST ON JBPM_LOG (VARIABLEINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_NODE ON JBPM_LOG (NODE_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_NODE ON JBPM_LOG (NODE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_LOG_PARENT ON JBPM_LOG (PARENT_);
Table "JBPM_LOG" not found; SQL statement:
--(optional)
CREATE INDEX FK_LOG_PARENT ON JBPM_LOG (PARENT_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_MODDEF_PROCDEF ON JBPM_MODULEDEFINITION (PROCESSDEFINITION_);
Table "JBPM_MODULEDEFINITION" not found; SQL statement:
--(optional)
CREATE INDEX FK_MODDEF_PROCDEF ON JBPM_MODULEDEFINITION (PROCESSDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TSKDEF_START ON JBPM_MODULEDEFINITION (STARTTASK_);
Table "JBPM_MODULEDEFINITION" not found; SQL statement:
--(optional)
CREATE INDEX FK_TSKDEF_START ON JBPM_MODULEDEFINITION (STARTTASK_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_MODINST_PRCINST ON JBPM_MODULEINSTANCE (PROCESSINSTANCE_);
Table "JBPM_MODULEINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_MODINST_PRCINST ON JBPM_MODULEINSTANCE (PROCESSINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASKMGTINST_TMD ON JBPM_MODULEINSTANCE (TASKMGMTDEFINITION_);
Table "JBPM_MODULEINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASKMGTINST_TMD ON JBPM_MODULEINSTANCE (TASKMGMTDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_DECISION_DELEG ON JBPM_NODE (DECISIONDELEGATION);
Table "JBPM_NODE" not found; SQL statement:
--(optional)
CREATE INDEX FK_DECISION_DELEG ON JBPM_NODE (DECISIONDELEGATION) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_NODE_PROCDEF ON JBPM_NODE (PROCESSDEFINITION_);
Table "JBPM_NODE" not found; SQL statement:
--(optional)
CREATE INDEX FK_NODE_PROCDEF ON JBPM_NODE (PROCESSDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_NODE_ACTION ON JBPM_NODE (ACTION_);
Table "JBPM_NODE" not found; SQL statement:
--(optional)
CREATE INDEX FK_NODE_ACTION ON JBPM_NODE (ACTION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_PROCST_SBPRCDEF ON JBPM_NODE (SUBPROCESSDEFINITION_);
Table "JBPM_NODE" not found; SQL statement:
--(optional)
CREATE INDEX FK_PROCST_SBPRCDEF ON JBPM_NODE (SUBPROCESSDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_NODE_SCRIPT ON JBPM_NODE (SCRIPT_);
Table "JBPM_NODE" not found; SQL statement:
--(optional)
CREATE INDEX FK_NODE_SCRIPT ON JBPM_NODE (SCRIPT_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_NODE_SUPERSTATE ON JBPM_NODE (SUPERSTATE_);
Table "JBPM_NODE" not found; SQL statement:
--(optional)
CREATE INDEX FK_NODE_SUPERSTATE ON JBPM_NODE (SUPERSTATE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_POOLEDACTOR_SLI ON JBPM_POOLEDACTOR (SWIMLANEINSTANCE_);
Table "JBPM_POOLEDACTOR" not found; SQL statement:
--(optional)
CREATE INDEX FK_POOLEDACTOR_SLI ON JBPM_POOLEDACTOR (SWIMLANEINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_PROCDEF_STRTSTA ON JBPM_PROCESSDEFINITION (STARTSTATE_);
Table "JBPM_PROCESSDEFINITION" not found; SQL statement:
--(optional)
CREATE INDEX FK_PROCDEF_STRTSTA ON JBPM_PROCESSDEFINITION (STARTSTATE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_PROCIN_PROCDEF ON JBPM_PROCESSINSTANCE (PROCESSDEFINITION_);
Table "JBPM_PROCESSINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_PROCIN_PROCDEF ON JBPM_PROCESSINSTANCE (PROCESSDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_PROCIN_ROOTTKN ON JBPM_PROCESSINSTANCE (ROOTTOKEN_);
Table "JBPM_PROCESSINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_PROCIN_ROOTTKN ON JBPM_PROCESSINSTANCE (ROOTTOKEN_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_PROCIN_SPROCTKN ON JBPM_PROCESSINSTANCE (SUPERPROCESSTOKEN_);
Table "JBPM_PROCESSINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_PROCIN_SPROCTKN ON JBPM_PROCESSINSTANCE (SUPERPROCESSTOKEN_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_RTACTN_PROCINST ON JBPM_RUNTIMEACTION (PROCESSINSTANCE_);
Table "JBPM_RUNTIMEACTION" not found; SQL statement:
--(optional)
CREATE INDEX FK_RTACTN_PROCINST ON JBPM_RUNTIMEACTION (PROCESSINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_RTACTN_ACTION ON JBPM_RUNTIMEACTION (ACTION_);
Table "JBPM_RUNTIMEACTION" not found; SQL statement:
--(optional)
CREATE INDEX FK_RTACTN_ACTION ON JBPM_RUNTIMEACTION (ACTION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_SWL_ASSDEL ON JBPM_SWIMLANE (ASSIGNMENTDELEGATION_);
Table "JBPM_SWIMLANE" not found; SQL statement:
--(optional)
CREATE INDEX FK_SWL_ASSDEL ON JBPM_SWIMLANE (ASSIGNMENTDELEGATION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_SWL_TSKMGMTDEF ON JBPM_SWIMLANE (TASKMGMTDEFINITION_);
Table "JBPM_SWIMLANE" not found; SQL statement:
--(optional)
CREATE INDEX FK_SWL_TSKMGMTDEF ON JBPM_SWIMLANE (TASKMGMTDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_SWIMLANEINST_TM ON JBPM_SWIMLANEINSTANCE (TASKMGMTINSTANCE_);
Table "JBPM_SWIMLANEINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_SWIMLANEINST_TM ON JBPM_SWIMLANEINSTANCE (TASKMGMTINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_SWIMLANEINST_SL ON JBPM_SWIMLANEINSTANCE (SWIMLANE_);
Table "JBPM_SWIMLANEINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_SWIMLANEINST_SL ON JBPM_SWIMLANEINSTANCE (SWIMLANE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASK_STARTST ON JBPM_TASK (STARTSTATE_);
Table "JBPM_TASK" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASK_STARTST ON JBPM_TASK (STARTSTATE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASK_PROCDEF ON JBPM_TASK (PROCESSDEFINITION_);
Table "JBPM_TASK" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASK_PROCDEF ON JBPM_TASK (PROCESSDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASK_ASSDEL ON JBPM_TASK (ASSIGNMENTDELEGATION_);
Table "JBPM_TASK" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASK_ASSDEL ON JBPM_TASK (ASSIGNMENTDELEGATION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASK_SWIMLANE ON JBPM_TASK (SWIMLANE_);
Table "JBPM_TASK" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASK_SWIMLANE ON JBPM_TASK (SWIMLANE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASK_TASKNODE ON JBPM_TASK (TASKNODE_);
Table "JBPM_TASK" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASK_TASKNODE ON JBPM_TASK (TASKNODE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASK_TASKMGTDEF ON JBPM_TASK (TASKMGMTDEFINITION_);
Table "JBPM_TASK" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASK_TASKMGTDEF ON JBPM_TASK (TASKMGMTDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TSK_TSKCTRL ON JBPM_TASK (TASKCONTROLLER_);
Table "JBPM_TASK" not found; SQL statement:
--(optional)
CREATE INDEX FK_TSK_TSKCTRL ON JBPM_TASK (TASKCONTROLLER_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASKACTPL_TSKI ON JBPM_TASKACTORPOOL (TASKINSTANCE_);
Table "JBPM_TASKACTORPOOL" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASKACTPL_TSKI ON JBPM_TASKACTORPOOL (TASKINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TSKACTPOL_PLACT ON JBPM_TASKACTORPOOL (POOLEDACTOR_);
Table "JBPM_TASKACTORPOOL" not found; SQL statement:
--(optional)
CREATE INDEX FK_TSKACTPOL_PLACT ON JBPM_TASKACTORPOOL (POOLEDACTOR_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TSKCTRL_DELEG ON JBPM_TASKCONTROLLER (TASKCONTROLLERDELEGATION_);
Table "JBPM_TASKCONTROLLER" not found; SQL statement:
--(optional)
CREATE INDEX FK_TSKCTRL_DELEG ON JBPM_TASKCONTROLLER (TASKCONTROLLERDELEGATION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TSKINS_PRCINS ON JBPM_TASKINSTANCE (PROCINST_);
Table "JBPM_TASKINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_TSKINS_PRCINS ON JBPM_TASKINSTANCE (PROCINST_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASKINST_TMINST ON JBPM_TASKINSTANCE (TASKMGMTINSTANCE_);
Table "JBPM_TASKINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASKINST_TMINST ON JBPM_TASKINSTANCE (TASKMGMTINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASKINST_TOKEN ON JBPM_TASKINSTANCE (TOKEN_);
Table "JBPM_TASKINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASKINST_TOKEN ON JBPM_TASKINSTANCE (TOKEN_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASKINST_SLINST ON JBPM_TASKINSTANCE (SWIMLANINSTANCE_);
Table "JBPM_TASKINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASKINST_SLINST ON JBPM_TASKINSTANCE (SWIMLANINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TASKINST_TASK ON JBPM_TASKINSTANCE (TASK_);
Table "JBPM_TASKINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_TASKINST_TASK ON JBPM_TASKINSTANCE (TASK_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TOKEN_SUBPI ON JBPM_TOKEN (SUBPROCESSINSTANCE_);
Table "JBPM_TOKEN" not found; SQL statement:
--(optional)
CREATE INDEX FK_TOKEN_SUBPI ON JBPM_TOKEN (SUBPROCESSINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TOKEN_PROCINST ON JBPM_TOKEN (PROCESSINSTANCE_);
Table "JBPM_TOKEN" not found; SQL statement:
--(optional)
CREATE INDEX FK_TOKEN_PROCINST ON JBPM_TOKEN (PROCESSINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TOKEN_NODE ON JBPM_TOKEN (NODE_);
Table "JBPM_TOKEN" not found; SQL statement:
--(optional)
CREATE INDEX FK_TOKEN_NODE ON JBPM_TOKEN (NODE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TOKEN_PARENT ON JBPM_TOKEN (PARENT_);
Table "JBPM_TOKEN" not found; SQL statement:
--(optional)
CREATE INDEX FK_TOKEN_PARENT ON JBPM_TOKEN (PARENT_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TKVARMAP_TOKEN ON JBPM_TOKENVARIABLEMAP (TOKEN_);
Table "JBPM_TOKENVARIABLEMAP" not found; SQL statement:
--(optional)
CREATE INDEX FK_TKVARMAP_TOKEN ON JBPM_TOKENVARIABLEMAP (TOKEN_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TKVARMAP_CTXT ON JBPM_TOKENVARIABLEMAP (CONTEXTINSTANCE_);
Table "JBPM_TOKENVARIABLEMAP" not found; SQL statement:
--(optional)
CREATE INDEX FK_TKVARMAP_CTXT ON JBPM_TOKENVARIABLEMAP (CONTEXTINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TRANSITION_FROM ON JBPM_TRANSITION (FROM_);
Table "JBPM_TRANSITION" not found; SQL statement:
--(optional)
CREATE INDEX FK_TRANSITION_FROM ON JBPM_TRANSITION (FROM_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TRANS_PROCDEF ON JBPM_TRANSITION (PROCESSDEFINITION_);
Table "JBPM_TRANSITION" not found; SQL statement:
--(optional)
CREATE INDEX FK_TRANS_PROCDEF ON JBPM_TRANSITION (PROCESSDEFINITION_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_TRANSITION_TO ON JBPM_TRANSITION (TO_);
Table "JBPM_TRANSITION" not found; SQL statement:
--(optional)
CREATE INDEX FK_TRANSITION_TO ON JBPM_TRANSITION (TO_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_VARACC_PROCST ON JBPM_VARIABLEACCESS (PROCESSSTATE_);
Table "JBPM_VARIABLEACCESS" not found; SQL statement:
--(optional)
CREATE INDEX FK_VARACC_PROCST ON JBPM_VARIABLEACCESS (PROCESSSTATE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_VARACC_SCRIPT ON JBPM_VARIABLEACCESS (SCRIPT_);
Table "JBPM_VARIABLEACCESS" not found; SQL statement:
--(optional)
CREATE INDEX FK_VARACC_SCRIPT ON JBPM_VARIABLEACCESS (SCRIPT_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_VARACC_TSKCTRL ON JBPM_VARIABLEACCESS (TASKCONTROLLER_);
Table "JBPM_VARIABLEACCESS" not found; SQL statement:
--(optional)
CREATE INDEX FK_VARACC_TSKCTRL ON JBPM_VARIABLEACCESS (TASKCONTROLLER_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_VARINST_PRCINST ON JBPM_VARIABLEINSTANCE (PROCESSINSTANCE_);
Table "JBPM_VARIABLEINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_VARINST_PRCINST ON JBPM_VARIABLEINSTANCE (PROCESSINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_VARINST_TKVARMP ON JBPM_VARIABLEINSTANCE (TOKENVARIABLEMAP_);
Table "JBPM_VARIABLEINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_VARINST_TKVARMP ON JBPM_VARIABLEINSTANCE (TOKENVARIABLEMAP_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_VARINST_TK ON JBPM_VARIABLEINSTANCE (TOKEN_);
Table "JBPM_VARIABLEINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_VARINST_TK ON JBPM_VARIABLEINSTANCE (TOKEN_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_BYTEINST_ARRAY ON JBPM_VARIABLEINSTANCE (BYTEARRAYVALUE_);
Table "JBPM_VARIABLEINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_BYTEINST_ARRAY ON JBPM_VARIABLEINSTANCE (BYTEARRAYVALUE_) [42102-186] 42S02/42102 (Aide)

 --(optional)
CREATE INDEX FK_VAR_TSKINST ON JBPM_VARIABLEINSTANCE (TASKINSTANCE_);
Table "JBPM_VARIABLEINSTANCE" not found; SQL statement:
--(optional)
CREATE INDEX FK_VAR_TSKINST ON JBPM_VARIABLEINSTANCE (TASKINSTANCE_) [42102-186] 42S02/42102 (Aide)

 --(optional);
Nombre de modifications: 0
(0 ms)

- sol
Where are the JBPM tables created? from which script?

AlfrescoPostCreate-JBPM-varinst-indexes.sql
-- 
-- Title:      Upgrade to V3.4 - Add indexes for jbpm foreign keys
-- Database:   Generic
-- Since:      V3.4 schema 4206
-- Author:     dward
-- 
-- Please contact support@alfresco.com if you need assistance with the upgrade.
-- 

CREATE INDEX IDX_VARINST_STRVAL ON JBPM_VARIABLEINSTANCE (NAME_, CLASS_, STRINGVALUE_, TOKENVARIABLEMAP_);
Table "JBPM_VARIABLEINSTANCE" not found; SQL statement:
-- 
-- Title:      Upgrade to V3.4 - Add indexes for jbpm foreign keys
-- Database:   Generic
-- Since:      V3.4 schema 4206
-- Author:     dward
-- 
-- Please contact support@alfresco.com if you need assistance with the upgrade.
-- 

CREATE INDEX IDX_VARINST_STRVAL ON JBPM_VARIABLEINSTANCE (NAME_, CLASS_, STRINGVALUE_, TOKENVARIABLEMAP_) [42102-186] 42S02/42102 (Aide)

 --(optional)

-- 
-- Record script finish
-- 
DELETE FROM alf_applied_patch WHERE id = 'patch.db-V3.4-JBPM-varinst-indexes';
Nombre de modifications: 0
(0 ms)

INSERT INTO alf_applied_patch
  (id, description, fixes_from_schema, fixes_to_schema, applied_to_schema, target_schema, applied_on_date, applied_to_server, was_executed, succeeded, report)
  VALUES
  (
    'patch.db-V3.4-JBPM-varinst-indexes', 'Manually executed script upgrade to add FK indexes for JBPM',
     0, 6016, -1, 6017, null, 'UNKOWN', ${TRUE}, ${TRUE}, 'Script completed'
   );
Syntax error in SQL statement "INSERT INTO alf_applied_patch
  (id, description, fixes_from_schema, fixes_to_schema, applied_to_schema, target_schema, applied_on_date, applied_to_server, was_executed, succeeded, report)
  VALUES
  (
    'patch.db-V3.4-JBPM-varinst-indexes', 'Manually executed script upgrade to add FK indexes for JBPM',
     0, 6016, -1, 6017, null, 'UNKOWN', ${TRUE}, ${TRUE}, 'Script completed'
   [*])" [42000-186] 42000/42000 (Aide)

