This project made to help with debugging problem with Ignite 2.7.0 in Karaf 4.2.0.

During initial start this configuration performs fine (see karaf_log_install.log for full log). 

    karaf@root()> feature:list | grep "Started"
    ...
    ki-test-core                             │ 0.0.1            │ x        │ Started     │ ki-test-features                  │ graphql core sandbox
    ignite-core                              │ 2.7.0            │          │ Started     │ ignite                            │ Apache Ignite :: Core
    ...
    
    karaf@root()> bundle:list | grep "ignite"
     85 │ Active   │  80 │ 2.7.0              │ ignite-core, Fragments: 86
     86 │ Resolved │  80 │ 2.7.0              │ ignite-osgi, Hosts: 85

But when I do restart of Karaf, it won't start.

There's exception in log (see karaf_log_restart.log for full log)

    2019-01-29T16:14:27,448 | ERROR | FelixDispatchQueue | ki-test-core                     | 70 - ki-test-core - 0.0.1 | FrameworkEvent ERROR - ki-test-core
    org.osgi.framework.BundleException: Unable to resolve ki-test-core [70](R 70.0): missing requirement [ki-test-core [70](R 70.0)] osgi.wiring.package; (&(osgi.wiring.package=org.apache.ignite.osgi.classloaders)(version>=2.7.0)(!(version>=3.0.0))) [caused by: Unable to resolve org.apache.ignite.ignite-osgi [86](R 86.0): missing requirement [org.apache.ignite.ignite-osgi [86](R 86.0)] osgi.wiring.host; (&(osgi.wiring.host=org.apache.ignite.ignite-core)(bundle-version>=0.0.0))] Unresolved requirements: [[ki-test-core [70](R 70.0)] osgi.wiring.package; (&(osgi.wiring.package=org.apache.ignite.osgi.classloaders)(version>=2.7.0)(!(version>=3.0.0)))]
        at org.apache.felix.framework.Felix.resolveBundleRevision(Felix.java:4149) ~[?:?]
        at org.apache.felix.framework.Felix.startBundle(Felix.java:2119) ~[?:?]
        at org.apache.felix.framework.Felix.setActiveStartLevel(Felix.java:1373) ~[?:?]
        at org.apache.felix.framework.FrameworkStartLevelImpl.run(FrameworkStartLevelImpl.java:308) ~[?:?]
        at java.lang.Thread.run(Thread.java:844) [?:?]
        
And, of course, bundle is not resolved and Ignite is not started

    karaf@root()> bundle:list | grep "ignite"
     85 │ Active    │  80 │ 2.7.0              │ ignite-core
     86 │ Installed │  80 │ 2.7.0              │ ignite-osgi