---
layout: single
title:  "How to set up a local database in PostgreSQL"
date:   2021-03-20 23:00:00 -0500
categories: Programming
comments: true
typora-root-url: ..
---

Recently, I have been working on a side project with my friend Saif which we have named social-media-analysis. Without getting too deep into it, we came up with the project to enhance our collaborative skills, and to familiarize ourselves with the PostgreSQL database, and the SQLAlchemy library in Python. Saif is joining my main project in 2 weeks so this project was planned to use a lot of the same tools used to write that app. This was also helpful to me because I could tackle PostgreSQL from the ground up and come back to my main project with that extra experience. 

After some time trying to make this work, I realized this was harder than I initially thought, and there were many steps involved to get setup for the first time, especially on Mac. I decided to document the process as I would no doubt need this again in the future. 

# Installing PostgreSQL

This step is pretty straight forward and documented well enough online. There are a few methods I have seen used to install PostgreSQL on Mac. The first method is to [install PostgreSQL with homebrew](https://wiki.postgresql.org/wiki/Homebrew). The second method is to go to the [PostgreSQL download page](https://www.postgresql.org/download/), and download the convenient mac installer. I personally went with this installer, which walked me through the installation step by step, allowing me to pick which parts of the app I wanted. During this process, the installer asks you to create a user/password, and advise which port the app should listen on.

# Creating a Local Database (localhost)

Next, open pgAdmin4. This will load either a web or desktop app with the servers listed on the left. Next, you need to right-click Servers, hover over "**Create**", and select "**Server**...". This will open a new window that is in the **General** tab by default. You can name it what you want, but I named mine Localhost. Next, click the Connection tab. In the address tab, you just want to enter "localhost". The port can be left the default value: **5432**. Finally, leave the username "postgres" unless you need to change this for some reason, and enter the pgAdmin master password. Voila! The server should be connected and running!

# Connecting to the database using Psycopg2 & SQLAlchemy

SQLAlchemy has a method called `create_engine` which can take in a connection string, and connect to the database using the information within it. 

To do this, create a file called `connection_string.txt`. 

You can then read this file by reading it using the built in `open` function in python, calling the `create_engine` function, and passing the connection string. Lastly, you can handle the common exceptions you may encounter. The full : 

```python
try:
	with open(connection_path, "r") as text_file:
		connection_string = text_file.read()
		engine = create_engine(connection_string, echo=True)
except FileNotFoundError:
	logging.error(f"The {connection_path} file is missing.")
except sqla_exc.ArgumentError:
	logging.error(f"Unable to connect to the database using the credentials in {connection_path}")
```

And voila! You should now be able to connect to the database you have created. Now you can use use the connection and use SQLAlchemy to read and write to your database.

# Conclusion

So this blog post was just about the basic setup, as it was giving me a bit of trouble and writing these blogs helps me cement the knowledge better than simply writing the code. If you are reading this, I hope it helps you with the initial setup as well. 

I will write a few more posts about things like inserting data into the database, and extracting data, as well as common exceptions to handle. Stay tuned for the rest of my posts about PostgreSQL!