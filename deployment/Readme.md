# To be written . . .
### Rough draft of things to be covered.
1. Cloning
2. Making changes and pushing
3. Data Pipeline (overview)

### Cloning and making changes to current workflow:  
The code pushes are auto enabled to make deployments on successful commits.
The current aws account with the full administrator access has to be availed and then necessary repository's http or ssh remote has to be copied.  
Once done, it has to be cloned in any local or remote environment using the credentials which will be available in the IAM section.

If code is being pulled, the application can be run in the local by setting up a similar local environment from the environment variables.  

Every time a change is commited and pushed to the repository, the codepipelines pulls the latest code from that branch.  
And pushes them into s3 for a version maintaince for reverting facility.  
The code is then pushed and verified successfully. If any changes, they are reverted back to the most recent running version.


### Data workflow:
All the applications such as op-api,op-ui, cdsp-ui, cdsp-api, master-registry-api, master-resgistry-ui follows a same workflow and is being pushed.

The workflow is being defined here:  
Please refer the image.  

The steps follow as below:  
1. 