# De Berardinis-Dossena TESI


<h2> Title: Event log quality-driven PPI assessment in collaborative processes </h2>
<h3> Contributors: </h3>
Alessia De Berardinis : alessia.deberardinis@mail.polimi.it <br>
Simone Dossena: simone1.dossena@mail.polimi.it <br>
<br>

This repository contains the coding scripts used for the testing part of the case study adopted to test the proposed framework.

<h3> Abstract of the thesis </h3>
Business processes represent a core asset of organizations and the evaluation of their effectiveness and efficiency is crucial to pursue strategic and operational goals. Process Performance Indicators (PPIs), which are a particular class of Key Performance Indicators (KPIs), are responsible for performing this assessment and, by capturing relevant aspects of business processes, provide managers with insights on how to improve the organization’s performance. 
In an increasingly dynamic and complex environment, companies need to work together to satisfy customers' needs and meet their expectations. The just mentioned collaboration leads to the establishment of an interorganizational business process, i.e., a set of joint activities carried out by the different organizations to achieve a common business goal. Interacting organizations exchange data with each other to evaluate the performance of the network and Process Performance Indicators represent the most appropriate tool for measuring business outcomes. Since interorganizational PPIs are calculated with data that originate from the log of different organizations, various errors may occur during the exchange of data and negatively affect the correctness of PPIs. The objective of this thesis is to assess the measurability and reliability of Process Performance Indicators based on the quality of data exchanged between actors in the context of interorganizational collaboration. For this purpose, we propose a framework that identifies the problems associated with the error-prone data sharing, investigates their impact on the measurability of PPIs through the data quality dimensions of accuracy, completeness, and consistency, and finally suggests possible remedial actions to mitigate these issues. In particular, the remediations aim at providing reliable values for erroneous or unavailable data stored in the event logs of the partners involved in the collaboration. To this end, we define a set of Reliable Process Indicators (RPIs) within the framework that are able to summarize the impact of the remedial actions on the value of the PPIs and on the related data quality dimensions, by also indicating the degree of corrective actions applied. 
A case study from the telecommunication industry concerning the installation of a DSL line is used to show a practical application of the methodology proposed.

<h3> Framework </h3>
The objective of our proposed framework is to assess the reliability of process performance indicators based on the quality of the data collected in the logs and required for their calculation. As mentioned in Section 2.2.3, several methodologies have been introduced to examine data quality of event logs and its impact on the calculation of PPIs, but this framework aims to go a step further by considering the context of interorganizational collaboration. 
If event logs handled by a single company can be subject to different types of errors, new issues can arise when a log contains data coming from outside the organization’s boundaries. This can happen because the organizations may not have direct access to this data but can use only what the other partners decide to make available.
Assessing the reliability of PPIs based on data shared among partners is important to understand how the way process data is currently logged can be improved to favor the integration among the actors and improve the way data is physically shared. 
Therefore, by understanding and analyzing the dependency between the quality of system logs and the measurability of PPIs, each actor of the collaboration might think about possible interventions in the data to improve their quality and consequently make the PPIs more reliable, i.e. with good measurability. Therefore, assessing PPI measurability provides significant indications about the reliability of the indicator. For this reason, the second goal of the proposed framework is to allow the companies involved in an interorganizational collaboration to think about how logged data can be treated to improve its quality, apply possible remediations – i.e. reliable values that replace missing or incorrect ones - and analyze their impact on the value of PPIs and of the related data quality dimensions. The last point is relevant as it allows organizations to assess whether the proposed remediations effectively improve the quality of PPIs without compromising the degree to which the event log is a faithful representation of the real world. 

<h3> Framework Validation </h3>
As anticipated, the proposed framework was validated in several steps, which are summarized in the validation plan below. <br> 
  1)	Simulate - through ProM software - the subprocesses to obtain the related event logs <br>
  2)	Combine the logs of the single subprocesses in order to obtain a unique event log containing the instances of the whole process. <br>
  3)	Define categories related to one or more data quality dimensions and assign PPIs – delineated in Chapter 4 - to them based on the main data type used for the calculation. Then, the most relevant PPIs - at least one for each category - are selected to apply the framework. <br>
  4)	Compute the value of the selected PPIs using the data contained in the event log obtained in step 3. <br>
  5)	Define the semantic rules that must be respected in the process and that involve the attributes needed for the computation of the PPIs. This step is useful with relation to the consistency-related errors applied in the following point. <br>
  6)	Perform the automatic error injection, that consists of inserting the errors defined in Chapter 4 in the unique event log.  Errors are injected based on the dimensions with which the PPIs are linked and - in case of consistency - based on the semantic rules defined. <br> 
  7)	Calculate again the value of the PPIs and of the related data quality dimensions. <br>
  8)	Apply the remediations defined in Chapter 4 to the values that result to be incorrect or missing due to errors. <br>
  9)	Recalculate the value of the PPIs using the remediations and compute again the related data quality dimensions. Define for each PPI and for each remediation applied, the corresponding RPI with information about the value, the data quality dimensions, the degree of remediations and the type of remediations applied. <br>
  10)	 Analyze the deviation between the value of the PPI calculated before and after the insertion of errors and remediations. <br>

<h3> Testing </h3>
The files contained in this GitHub repository implements the testing part of the steps defined above. In particular: <br>

<h5> ProcessBuilderFinale </h5>
It implements the generation of the final event log that contains the instances related to the flow of the whole process. <br>
After simulating the five subprocesses, it was necessary to combine all the event logs obtained into a single one containing the instances of the entire process and tracking the process flow. This procedure was performed only to facilitate the testing part, since, as already anticipated, in a realistic context each partner cannot see the whole process flow, but only the events that - in accordance with the monitoring agreement - are transmitted by the other parties.
Therefore, this operation of recombining the event logs of the different subprocesses was performed to create a unique workbench in which it was easier to perform the generation of realistic timestamps and the error injection.
Due to the large amount of tuples to be processed (more than 100k by summing the tuples of all subprocesses). The main task of the script is to physically move the tuples of the five event logs related to the subprocesses to a new and unique log according to the correct flow of the whole process. In addition, the script also performs the task of simulating and appending the various columns related to the event attributes needed to calculate the identified PPIs. Gradually, additional versions of the code were developed to implement the error injection. In particular, a specific version of the original code was implemented for each data quality dimension.
A general description of the code is reported: 
After importing the required libraries, the script begins uploading the five logs related to the subprocesses. All the operations on the various event logs were performed by exploiting the DataFrame structure available in Python, which is suitable for managing large amounts of structure data. Upon completion of log uploading, the script removes the columns of the logs that are no longer useful, that are 'startTime', 'completeTime', 'concept:simulated' and 'simulated:logProbability'. It is important to specify that both the time-related columns are eliminated, as the temporal dimensions of the event logs are fully handled by the script. The unique event log is built starting from an empty DataFrame, into which the tuples coming from the different event logs are moved according to the correct sequence of events that characterize the flow of the whole process. Specifically, the script goes into each of the five logs, identifies the correct rows to move and selects them. The condition used by the code to understand how to stop in each log is the name of the last activity in that step to be selected. It is important to point out that, by deciding the process flow beforehand, micro variations of the process cannot be represented with this algorithm. However, all the main flow branches of the process are certainly represented, which are:<br>
a.	Unavailability of order <br>
b.	Availability of the order, completion of it with/without eligible/not eligible complaint sent by the subscriber and payment with/without the solicitation from the service agent to the customer.
<br>

<h5> EventLogs </h5>
It is the folder that containts the simulated event logs of the subprocesses used in ProcessBuilderFinale

<h5> ErrorInjection </h5>
This folder contains the scripts to insert errors of different types in the event log resulted by running ProcessBuilderFinale <br>
From a practical point of view, error injection was implemented with four short Python scripts, one for each data quality dimension and one to insert errors related to different dimensions considered together. The script that inserts the errors behaves as follows: for each attribute where the insertion of an error is possible, the decision whether to actually insert the error is made randomly by the script. When it is inserted, the counter for the total number of errors is decreased by one. The procedure is repeated until the number of errors is reduced to zero.
<br>

<h3> Tools </h3>
The scripts has been coded using Jupyter Notebooks. Then, they have been runned in Visual Studio Code using a Python Interpreter (version 3.9.12)

