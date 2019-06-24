[![Deploy](https://get.pulumi.com/new/button.svg)](https://app.pulumi.com/new)

# Azure Web Server example in Python

This example deploys an Azure Virtual Machine and starts a HTTP server on it.

## Prerequisites

1. [Install Pulumi](https://pulumi.io/install/)
1. [Configure Pulumi for Azure](https://pulumi.io/quickstart/azure/setup.html)
1. [Configure Pulumi for Python](https://pulumi.io/reference/python.html)

## Deploying and running the program

1. Set up a virtual Python environment and install dependencies

    ```
    $ virtualenv -p python3 venv
    $ source venv/bin/activate
    $ pip install -r requirements.txt
    ```

1. Create a new stack:

    ```
    $ pulumi stack init azure-py-webserver
    ```

1. Set the Azure environment:

    ```
    $ pulumi config set azure:environment public
    ```

1. Set the required configuration for this example. This example requires you to supply a username and password to
the virtual machine that we are going to create.

    ```
    $ pulumi config set azure-web:username myusername
    ```

    The password is a secret, so we can ask Pulumi to encrypt the configuration:

    ```
    $ pulumi config set --secret azure-web:password Hunter2hunter2
    ```

1. Run `pulumi up` to preview and deploy the changes:

    ```
    $ pulumi update
    Previewing update (azuredev):

        Type                               Name                         Plan       
    +   pulumi:pulumi:Stack                azure-py-webserver-azuredev  create     
    +   ├─ azure:core:ResourceGroup        server                       create     
    +   ├─ azure:network:VirtualNetwork    server-network               create     
    +   ├─ azure:network:PublicIp          server-ip                    create     
    +   ├─ azure:network:Subnet            server-subnet                create     
    +   ├─ azure:network:NetworkInterface  server-nic                   create     
    +   └─ azure:compute:VirtualMachine    server-vm                    create     
    
    Resources:
        + 7 to create

    Do you want to perform this update? yes
    Updating (azuredev):

        Type                               Name                         Status      
    +   pulumi:pulumi:Stack                azure-py-webserver-azuredev  created     
    +   ├─ azure:core:ResourceGroup        server                       created     
    +   ├─ azure:network:VirtualNetwork    server-network               created     
    +   ├─ azure:network:PublicIp          server-ip                    created     
    +   ├─ azure:network:Subnet            server-subnet                created     
    +   ├─ azure:network:NetworkInterface  server-nic                   created     
    +   └─ azure:compute:VirtualMachine    server-vm                    created     
    
    Outputs:
        public_ip: "137.117.15.111"

    Resources:
        + 7 created

    Duration: 2m55s

    Permalink: https://app.pulumi.com/swgillespie/azure-py-webserver/azuredev/updates/3
    ```

1. Get the IP address of the newly-created instance from the stack's outputs: 

    ```
    $ pulumi stack output public_ip
    137.117.15.111
    ```

1. Check to see that your server is now running:

    ```
    $ curl http://$(pulumi stack output public_ip)
    Hello, World!
    ```

1. Destroy the stack:

    ```
    ▶ pulumi destroy --yes
    Previewing destroy (azuredev):

        Type                               Name                         Plan       
    -   pulumi:pulumi:Stack                azure-py-webserver-azuredev  delete     
    -   ├─ azure:compute:VirtualMachine    server-vm                    delete     
    -   ├─ azure:network:NetworkInterface  server-nic                   delete     
    -   ├─ azure:network:Subnet            server-subnet                delete     
    -   ├─ azure:network:PublicIp          server-ip                    delete     
    -   ├─ azure:network:VirtualNetwork    server-network               delete     
    -   └─ azure:core:ResourceGroup        server                       delete     
    
    Resources:
        - 7 to delete

    Destroying (azuredev):

        Type                               Name                         Status      
    -   pulumi:pulumi:Stack                azure-py-webserver-azuredev  deleted     
    -   ├─ azure:compute:VirtualMachine    server-vm                    deleted     
    -   ├─ azure:network:NetworkInterface  server-nic                   deleted     
    -   ├─ azure:network:Subnet            server-subnet                deleted     
    -   ├─ azure:network:VirtualNetwork    server-network               deleted     
    -   ├─ azure:network:PublicIp          server-ip                    deleted     
    -   └─ azure:core:ResourceGroup        server                       deleted     
    
    Resources:
        - 7 deleted

    Duration: 3m49s

    Permalink: https://app.pulumi.com/swgillespie/azure-py-webserver/azuredev/updates/4
    ```