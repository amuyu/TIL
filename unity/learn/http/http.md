

네트워크 클라이언트와 서버
https://docs.unity3d.com/kr/2018.4/Manual/UNetClientServer.html


HTTP 서버로 양식 보내기 - UnityWebRequest
https://docs.unity3d.com/kr/2018.4/Manual/UnityWebRequest-HLAPI.html
https://docs.unity3d.com/kr/2018.4/Manual/UnityWebRequest-SendingForm.html
```
using UnityEngine;
using UnityEngine.Networking;
using System.Collections;
 
public class MyBehavior : MonoBehaviour {
    void Start() {
        StartCoroutine(Upload());
    }
 
    IEnumerator Upload() {
        List<IMultipartFormSection> formData = new List<IMultipartFormSection>();
        formData.Add( new MultipartFormDataSection("field1=foo&field2=bar") );
        formData.Add( new MultipartFormFileSection("my file data", "myfile.txt") );

        UnityWebRequest www = UnityWebRequest.Post("http://www.my-server.com/myform", formData);
        yield return www.SendWebRequest();
 
        if(www.isNetworkError || www.isHttpError) {
            Debug.Log(www.error);
        }
        else {
            Debug.Log("Form upload complete!");
        }
    }
}
```


유니티 Rest API 통신 UnityWebRequest
https://doobudubu.tistory.com/229