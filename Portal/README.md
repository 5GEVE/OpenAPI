## GENERAL
The EVE_PORTAL_API.postman_collection.json aims to provide examples of the REST calls exposed by the 5GEVE Portal. The collection is divided in two folders: (i) "catalogue" containing all the REST calls available to retrieve descriptors from the 5GEVE Portal CATALOGUE (i.e. Context Blueprints, Vertical Service Blueprints, Experiment Blueprints, etc); (ii) "lcm" containing the REST calls to manage the lifecycle of the experiment. In order to use it is enough to import the file in Postman (tested in Postman Version 7.28.0), and set the required environment variables for each request. The following sections detail the specifics of each request.


## LOGIN

This request interacts with the 5GEVE PORTAL RBAC system to authenticate and retrieve the roles of the user, and therefore must be executed before the catalogue/lcm calls. The body of this request uses two postman environment variables: (i) "user", which must be filled in with the email of the registered user; (ii) "password" which should contain the correspondent password. If the authentication succeeds, a Postman test script will automatically read the response and populate the "access_token" environment variable. This latter variable is automatically included in the headers of all the other REST calls. 

## catalogue

### GET_ALL_*:

These requests allow to retrieve a list of all descriptors of each type available (EXPBs/CTXBs/VSBs/TCBs/EXPDs/TCDs/VSDs/CTXDs). No particular parameter is needed for these requests.

### GET_*:

These requests allow to retrieve a specific descriptor using the id of the descriptor. The id is specified on the URL of the request, which in this Postman collection is filled with the value of specific environment variables ("expb_id","vsb_id", "ctxb_id", "tcb_id", "vsd_id", "expd_id", "ctxd_id" and  "tcd_id")  

## lcm
### CREATE_EXPERIMENT:

This request allows to request an experiment. If the request succeeds, a Postman test script will automatically fill the "exp_id" environment variable which is used by other lcm requests. The body uses the following environment variables:

	- "expd_id": as before, this variable indicates the specific Experiment Descriptor to be used in the experiment.
	- "experiment_name": the descriptive name to be used for the experiment.
	- "startTime"/"stopTime": these two variables indicate the timeslot requested for the experiment. The current request has a Postman pre-request script that automatically sets values for these two variables. 
	- "target_site": indicates the target site for the experiment. Must be filled with one of the following values: ITALY_TURIN, SPAIN_5TONIC, FRANCE_PARIS, FRANCE_NICE, FRANCE_RENNES, GREECE_ATHENS
	- "experiment_usecase" : the use case associated with the experiment.

### GET_ALL_EXPERIMENTS 
Retrieve all the experiments of the user. No parameter is needed for this request. 

### GET_EXPERIMENT_BY_ID 
Retrieve an experiment using the id. This request uses the "exp_id" environment variable to fill the experiment id parameter in the URL of the request. 

### GET_EXPERIMENT_BY_EXPD
Retrieve the experiments associated with an EXPD. This request uses the "expd_id" environment variable to fill the expd id parameter in the URL of the request. 
### DEPLOY_EXPERIMENT
Triggers the deployment of one experiment. The REST call requires the experiment id, which is passed in the URL using the value of the "exp_id" environment variable.
### EXECUTE_EXPERIMENT
Triggers the execution of one experiment. The REST call requires the experiment id, which is passed in the URL using the value of the "exp_id" environment variable. The body of this request allows to override the values for the parameters of the testcase using a map of parameterid-value for each testdescriptor id. The body also should contain a name for the execution, which in this case is filled with the value of the "execution_name" environment variable. 
### TERMINATE_EXPERIMENT
Triggers the termination of one experiment. The REST call requires the experiment id, which is passed in the URL using the value of the "exp_id" environment variable.


