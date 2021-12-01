## Cloud-Init Config example for Azure

### To launch a VM using Azure CLI

#### Make sure your region supports the vm sizing
Example with Korea Central region
```
az vm image list --offer UbuntuLTS --all --output table
az vm list-sizes --location koreacentral --output table | grep b1ms
```

Save the 10_dynatrace.cfg somewhere on your local machine as text file, eg:

`10_dynatrace.cfg` --> `/opt/azure-custom-data-template.txt`

#### Create the VM with custom-data payload
Assuming you already have a resource-group created

```
az vm create --resource-group <<YOUR-RESOURCE-GROUP>> --name Demo-Nginx01 --image UbuntuLTS --size Standard_B1ms --admin-username demo --generate-ssh-keys --custom-data /opt/azure-custom-data-template.txt

az vm open-port --port 80 --resource-group <<YOUR-RESOURCE-GROUP>>  --name Demo-Nginx01
```
This will create a VM with a Node.js/Nginx "Hello world!" app running and Dynatrace will have instrumented the processes automatically.

#### To clean up the VM later:
```
az vm delete --resource-group <<YOUR-RESOURCE-GROUP>> --name Demo-Nginx01
```
