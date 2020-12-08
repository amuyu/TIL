# JAVA Version Control 
```
javahome_usage() { 
    echo "javahome - switch to different JDK version" 
    echo "Usage: javahome [-h] [-v VERSION]" 
    echo 
    echo " -h : display usage" 
    echo " -v : specific JDK version to switch" 
    echo 
    echo "Examples: " 
    echo "># javahome -v 1.8 : switches to JDK8" 
    echo "># javahome -v 12 : switches to JDK12" 
    echo "># javahome : display all installed JDK and display current JDK" 
} 

javahome () {
    if [ "$1" = "-h" ] ; then 
        javahome_usage 
    fi 
    if [ "$#" -eq 0 ] ; then 
        /usr/libexec/java_home -V 
    fi 
    if [ "$#" -eq 2 ] && [ "$1" = "-v" ] ; then 
        export JAVA_HOME=`/usr/libexec/java_home $@` 
        echo "Setting JAVA_HOME:" $JAVA_HOME 
        echo 
        echo "Added JAVA_HOME/bin to PATH" PATH=$PATH:$JAVA_HOME/bin 
        echo $PATH 
        echo java -version 
    fi 
}
```
https://woolbro.tistory.com/2