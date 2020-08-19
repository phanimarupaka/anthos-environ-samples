anthos-environ-samples
==================================================

# NAME

  anthos-environ-samples

# SYNOPSIS

  Get and instantiate package on Day 1:
    
    $ kpt pkg get git@github.com:phanimarupaka/anthos-environ-samples.git .
    $ cd anthos-environ-samples
    $ kpt cfg tree .

    // Future: list-setters will recursively list setters in subpackages
    $ kpt cfg list-setters .
    cloud-run/
         NAME           VALUE        SET BY   DESCRIPTION   COUNT   REQUIRED  
      membership   [gke-cluster-1]                          1       No        
      
    config-management/  
                      NAME                                         VALUE                          SET BY   DESCRIPTION   COUNT   REQUIRED  
      anthos.configmanagement.git.syncrepo   git@github.com:cydu-cloud/user-acm-config-repo.git                          1       Yes       
      membership                             [gke-cluster-1]                                                             1       No        
      
    gke-cluster-1/  
                    NAME                      VALUE        SET BY   DESCRIPTION   COUNT   REQUIRED  
      gcloud.container.cluster.location   us-central1-c                           2       No        
      gcloud.container.cluster.name       gke-cluster-1                           4       No        
      gcloud.container.nodepool.name      gke-nodepool-1                          1       No        
      gcloud.core.project                 PROJECT_ID                              3       No        
      gcloud.project.projectNumber        PROJECT_NUMBER                          1       No        
    
    service-mesh/
         NAME           VALUE        SET BY   DESCRIPTION   COUNT   REQUIRED  
      membership   [gke-cluster-1]                          1       No       
    
    # anthos.configmanagement.git.syncrepo is required as mentioned in list-setters result
    $ kpt cfg set config-management anthos.configmanagement.git.syncrepo git@github.com:cydu-cloud/user-acm-config-repo.git
    $ kpt fn run .

   Add more clusters and extend the membership on Day n:
    
    // Future: adds dependency to Kptfile
    $ kpt pkg add-subpackage . git@github.com:phanimarupaka/anthos-environ-samples.git/gke-cluster-1 --local-dest-dir gke-cluster-2
    
    // Future: fetches the dependency specified above into gke-cluster-2
    $ kpt pkg update .
    
    $ kpt cfg set gke-cluster-2 gcloud.container.cluster.name gke-cluster-2
    $ kpt cfg set gke-cluster-2 gcloud.container.nodepool.name us-nodepool-2
    
    // Future: Run the set command on all the subpackages with -R flag
    // only the packages with the setter will be set
    $ kpt cfg set . membership gke-cluster-1 gke-cluster-2 -R
    set 1 fields in cloud-run/
    set 1 fields in config-management/
    set 1 fields in service-mesh/
    setter membership is not found in gke-cluster-1/
    setter membership is not found in gke-cluster-2/
    setter membership is not found in .
    
    $ kpt fn run .
    
  The packages on day n after the operations looks like this:
  
  https://github.com/phanimarupaka/consumed-anthos-environ-samples
    

