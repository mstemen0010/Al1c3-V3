# Al1c3-V3
Third variation on the Java FX Alice polymorphic browser

What is a polymorphic browswer one may ask? Simple answer, a browswer that is able to change its underlying engine (or version
of HttpClient) on the fly. This is done through a combination an enumations that contains values that are ultimately class names
and reflection. this results in typed/type safe driven reflection at runtime (very polymorphic indeed) and may represent a new 
type of entity in OOD. Alice was made to help explore a security threat cause by various versions of the Apaches HttpClient
and to be able to explore and widen the exploit it can cause on the fly. The user simply can change the version of HTTPClient 
(each version had various different and subtle nuanances to them) by selecting from a drop down. In theory this design could be 
used in a variety of ways outside of this application especically when concidering the fact that Interfaces can have enumerations 
on them as well (a fact little know in the Java world it seems) So an Java program could implement different interfaces on the fly 
through reflection as well and those interfaces could expose the running program to other classes (typed by way of the enumeration)
which could be spun up and loaded at runtime as well. This program used "proxy" classes to serve as an adaptor to other classes. 
It is worth noting that the reflect to "spin' up the classes through reflection is done via a ProtoType (an Interface) which 
also reflects the classes into existence via the typed enumerations (since enumerations are inner classes in java, you can have
methodology on said enumerations: 

  enum AliceClientType {
 23 
 24         Undefined("Undefined", null, null ),
 25         HttpClient31("HttpClient 3.1", HttpClient.class, AliceProtoClient31.class ),
 26         HttpClient42("HttpClient 4.2", DefaultHttpClient.class, AliceProtoClient42.class );
 27         String friendlyName;
 28         Class clientClass;
 29         Object currentClientObject;
 30         AliceProtoClient currentClient;
 31         Class adaptorClass;
 32 
 33         AliceClientType(String newFriendlyName, Class clientClass, Class adaptorClass ) {
 34             this.friendlyName = newFriendlyName;
 35             this.clientClass = clientClass;
 36             this.adaptorClass = adaptorClass;
 37             if (this.clientClass != null) {
 38                 spinClient();
 39             }
 40         }
.
.
.
.
.
.
      public AliceProtoClientInterface spinClient() {
 58             try {
 59                 // use refecttion
 60                 this.currentClientObject = this.clientClass.newInstance();
 61                 this.currentClient = (AliceProtoClient) this.adaptorClass.newInstance();
 62             } catch (InstantiationException ex) {
 63                 Logger.getLogger(AliceProtoClientInterface.class.getName()).log(Level.SEVERE, null, ex);
 64             } catch (IllegalAccessException ex) {
 65                 Logger.getLogger(AliceProtoClientInterface.class.getName()).log(Level.SEVERE, null, ex);
 66             }
 67             
 68             return this.currentClient;
 69             
 70         }
 
 
 
 
