# Application Express (APEX) List

This list just contains a list of resources I feel are useful.

## Videos

* [Beautiful UI in APEX 5.0](https://www.youtube.com/watch?v=2uBQF7wk3zg) - A conference talk by Shakeeb in 2015 introducing to the Universal Theme

## SQL Developer

### Decrypting passwords

There is an extension to aid in decypting passwords, and a couple of example code snippets. I took this project [maaaaz/sqldeveloperpassworddecryptor](https://github.com/maaaaz/sqldeveloperpassworddecryptor) and renamed it to `sdpd.py`. For version 4 of SQL Developer, you need to figure out the system id property, then call the script with that value and the password.

Ok, with that saved somewhere on your system, step 1. Find out the `db.system.id` property. This is stored in a file named `product-preferences.xml`, in a path that resembles: `$HOME/.sqldeveloper/system4.2.0.16.356.1154/o.sqldeveloper.12.2.1.16.356.1154` depending onthe particular version you have installed. Then, you can just run: `cat product-preferences | grep db.system`. The output will look like:

```bash
cat product-preferences.xml | grep db.system
   <value n="db.system.id" v="system-id"/>
```

Then next part is to decrypt the password. Connections are stored in a path that resembles: `$HOME/.sqldeveloper/system4.2.0.16.356.1154/o.jdeveloper.db.connection.13.0.0.1.42.161121.1801` in a file named `connections.xml`.

You will find the password in a node such as:

```xml
<Reference name="connection_name" className="oracle.jdeveloper.db.adapter.DatabaseProvider" xmlns="">
      <Factory className="oracle.jdevimpl.db.adapter.DatabaseProviderFactory1212"/>
      <RefAddresses>
         <StringRefAddr addrType="password">
            <Contents>encrypted-password</Contents>
         </StringRefAddr>
      <RefAddresses>
<Reference
```

Where encrypted-password is value to be passed into the program.

So with all that info, we can decrypt like so:

```
./sdpd.py -d system-id -p encrypted-password
sqldeveloperpassworddecryptor.py version 1.0

[+] encrypted password: encrypted-password
[+] db.system.id value: system-id

[+] decrypted password: original-password

```
