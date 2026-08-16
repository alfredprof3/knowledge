#type/HowTo #topic/Debian/Home #for/Debian 

# How to Create a Home Directory for an Existing User?
Source: [How to create a home directory for an existing user?](https://labex.io/questions/how-to-create-a-home-directory-for-an-existing-user-568988)

> [!steps]
> 1. Open your terminal.
>     
> 2. Run the following command, replacing `username` with the actual username and `/home/username` with the desired home directory path:
> 
> 	```bash
> 	sudo usermod -d /home/username -m username
> 	```
> 
> 	- `-d /home/username`: This specifies the new home directory path.
> 	- `-m`: This option moves any existing content from the old home directory to the new one (if applicable).
> 
> 	For example, to create a home directory for a user named `bob`, you would run:
> 
> 	```bash
> 	sudo usermod -d /home/bob -m bob
> 	```
> 
> 	After executing this command, you can verify that the home directory has been created by checking its existence:
> 
> 	```bash
> 	ls -ld /home/bob
> 	```
