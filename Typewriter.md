---
layout: page
permalink: /portfolio/samples/Typewriter
---
This code snippet is used to display text to a UI element within Unity one letter at a time. It is possible to control the speed of the text with a variable exposed to the editor and even change the string.

{% highlight csharp %}
public Text typewriterText;
 
string[] textForTypeWriter = new string[] { "This text will be printed character by the coroutine" };
 
int currentChar = 0; // Value used to check iterator i
  
void Awake()
{
    StartCoroutine(TypeWriter());
}
 
IEnumerator TypeWriter()
{
    for (int i = 0; i < textForTypeWriter[currentChar].Length + 1; i++) //While i < currentChar + 1
{
    typewriterText.text = textForTypeWriter[currentChar].Substring(0, i); //Retrieve the substring from this instace
    yield return new WaitForSeconds(.03f);                                //Every 0.3 seconds
}
}
{% endhighlight %}