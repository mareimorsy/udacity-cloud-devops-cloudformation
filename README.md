# Udacity Cloud DevOps - Pass the 2nd project the easy way

0. Delete all stacks if there any

1. Clone supporting material repo
```
git clone https://github.com/udacity/nd9991-c2-Infrastructure-as-Code-v1.git
```

2. open supporting material folder in vscode
```
code nd9991-c2-Infrastructure-as-Code-v1/supporting_material
```

3. open `servers.yml`

- Remove line `59`, `60`
- Change `ImageId` in line `60` to be `ami-0729e439b6769d6ab`
- Change `FromPort` and `ToPort` in lines `36`, `37` from `8080` to `80`
- Change `WebAppTargetGroup Port` in line `118` from `8080` to `80`
- Replace the UserData from line `53` to `56` with the following snippet
```
          #!/bin/bash
          apt-get update -y
          apt-get install apache2 -y
          systemctl start apache2.service
          cd /var/www/html
          echo "Udacity Demo Web Server Up and Running!" > index.html
```
- Add the following snippet to the end of the file
```
Outputs:

    WebAppLBDNSName:
        Description: add http:// to LB DNS Name
        Value: !Join ["", ["http://", !GetAtt WebAppLB.DNSName]]
        Export:
          Name: !Sub ${EnvironmentName}-LB-DNSName
```
4. Run The network stack
```
./create.sh my-network network.yml network-parameters.json
```
Wait until it's completed, then
5. Run the servers stack
```
./create.sh my-servers servers.yml server-parameters.json
```

6. Submit the following:
- Github repo contains your config
- Draw a diagram like that
![Diagram](https://raw.githubusercontent.com/mareimorsy/udacity-cloudformation/main/images/00-diagram.png)
- LoadBalancer DNS Name
    - CloudFormation -> servers stack -> output -> WebAppLBDNSName
![DNS Name](https://raw.githubusercontent.com/mareimorsy/udacity-cloudformation/main/images/04-alb-dns-output.png)
7. Don't forget to delete the stack once your project passes
