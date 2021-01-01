# CRD-operations-of-a-file-based-key-value-data-store
This is a file which can be exposed as a library that supports the basic CRD(create, read, write) operations. Data store is meant to local storage for one single process on single laptop

The data store will support the following :
1. It can be initialized using an optional file path. If one is not provided, it will reliably 
create itself in a reasonable location on the laptop.
2. A new key-value pair can be added to the data store using the Create operation. The key 
is always a string - capped at 32chars. The value is always a JSON object - capped at 
16KB.
3. If Create is invoked for an existing key, an appropriate error must be returned.
4. A Read operation on a key can be performed by providing the key, and receiving the 
value in response, as a JSON object.
5. A Delete operation can be performed by providing the key.
6. Every key supports setting a Time-To-Live property when it is created. This property is
optional. If provided, it will be evaluated as an integer defining the number of seconds 
the key must be retained in the data store. Once the Time-To-Live for a key has expired, 
the key will no longer be available for Read or Delete operations.
7. Appropriate error responses must always be returned to a client if it uses the data store in 
unexpected ways or breaches any limits
8. The file size never exceeds 1GB
9. The file is accessed by multi-threading


Supported OS: Any(Windows, unix, Mac, etc.)


Instruction to run commands:-

#here are the commands to demonstrate how to access and perform operations on a main file


#run the MODULE of MAIN FILE and import mainfile as a library 

import store as m
#importing the main file("store" is the name of the file I have used) as a library 


m.create("Faridabad",25)
--to create a key with key_name,value given and with no time-to-live property


m.create("Gurgaon",70,3600) 
--to create a key with key_name,value given and with time-to-live property value given(number of seconds)


m.read("Faridabad")
--it returns the value of the respective key in Jsonobject format 'key_name:value'


m.read("Gurgaon")
--it returns the value of the respective key in Jsonobject format if the TIME-TO-LIVE IS NOT EXPIRED else it returns an ERROR


m.create("Faridabad",50)
--it returns an ERROR since the key_name already exists in the database
-To overcome this error 
-either use modify operation to change the value of a key
-or use delete operation and recreate it


m.modify("Faridabad",55) 
--t replaces the initial value of the respective key with new value 

 
m.delete("Faridabad")
--it deletes the respective key and its value from the database(memory is also freed)

--we can access these using multiple threads like
t1=Thread(target=(create or read or delete),args=(key_name,value,timeout)) #as per the operation
t1.start()
t1.sleep()
t2=Thread(target=(create or read or delete),args=(key_name,value,timeout)) #as per the operation
t2.start()
t2.sleep()
--and so on upto tn

--the code also returns other errors like 
--"invalidkey" if key_length is greater than 32 or key_name contains any numeric,special characters etc.,
--"key doesnot exist" if key_name was mis-spelt or deleted earlier
--"File memory limit reached" if file memory exceeds 1GB


