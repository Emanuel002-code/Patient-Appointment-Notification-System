      Patient Appointment Notification System

The task was to build a Patient Appointment Notification System using IBM 
App Connect Enterprise (ACE), IBM MQ, and PostgreSQL/DB2. The system 
was required to receive appointment data from five different input formats: 
   • CSV 
   • HL7 
   • SAOP 
   • XML 
   • JSON 
Each format is processed by its own message flow, which converts the incoming 
data into a standard (canonical) JSON format. The messages are then sent 
through IBM MQ before being processed by a single persistence flow that stores 
the appointment information in the database.

-------------------------------------------------------------------------------------
Development Environment:

  >Java JDK 17 for compiling IBM ACE projects 
  >IBM App Connect Enterprise (ACE) for building the integration solution 
  >IBM MQ for reliable message queuing 
  >PostgreSQL/DB2 for storing appointment data 
  >ostman for testing REST APIs 
  >ODBC Data Source for connecting IBM ACE to the database

---------------------------------------------------------------------------------------
Database Configuration:

A database named CLINICDB was created with a schema named  CLINIC. 
The following tables were created: 

   > APPOINTMENTS – stores valid appointment records. 
   > APPT_DLQ_LOG – stores invalid or failed appointment records.

An ODBC Data Source named PostgreSQL35W was also configured          
establish    the connection between IBM ACE and the PostgreSQL database. 

---------------------------------------------------------------------------------------
IBM MQ Configuration:

A Queue Manager named QM1 was created with the following queues: 
   >APPT.INPUT.JSON – receives JSON REST requests. 
   >APPT.INPUT.XML – receives XML messages. 
   >APPT.INPUT.CSV – receives CSV batch records. 
   >APPT.CANONICAL – stores all normalised messages. 
   >APPT.DLQ – stores invalid or failed messages.

---------------------------------------------------------------------------------------
Message flows:

The following message flows were implemented: 
  >CSV_Batch_Flow – processes appointment data from CSV files received via FTP. 
  >HL7_V2_Flow – processes HL7 text files received via FTP. 
  >JSON_API_Flow – processes appointment data received through a REST API. 
  >XML_Ingest_Flow – processes XML appointment messages. 
  >SOAP_Flow – processes SOAP XML appointment messages. 
  >DL_Processing_Flow – processes messages from the Dead Letter Queue (DLQ) and attempts to repair invalid payloads. 
  >MQ_To_DB_Flow – consumes messages from the canonical queue and stores them in the database. 
  >Canonical Routing Flow – receives normalised messages from the different flows and forwards them to the canonical queue. 
  >Finally, an IBM ACE Integration Server was created to deploy and run all the message flows.

------------------------------------------------------------------------------------------
