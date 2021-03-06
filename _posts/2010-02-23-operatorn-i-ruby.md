---
layout: post
title: "||=-operatorn i Ruby"
tags: archived
language: sv
---
I Ruby, liksom de flesta andra programmeringsspråk, finns det något som kallas _abbreviated assignment_ (förkortad tilldelning). Det är operatorer som <code>+=</code> och <code>-=</code> som låter oss lägga till respektive dra ifrån värdet på en redan existerande variabel.

Om man är ny i Ruby-världen finns det risk för förvirring när man stöter på <code>||=</code> i andras källkod. Vad gör den och vad är den bra till? Jag tänkte förklara med ett exempel.

{% highlight ruby %}
puts @count
#=> nil

@count ||= 0
puts @count #=> 0
{% endhighlight %}

Okej, vad händer här egentligen? Vi börjar med en instansvariabel <code>@count</code> som är odefinierad (<code>nil</code>). Sedan använder vi <code>||=</code>-operatorn och <code>@count</code> är nu 0. Har vi bara gjort en vanlig tilldelning?

{% highlight ruby %}
@count ||= 32
puts @count #=> 0
{% endhighlight %}

Det verkar inte så, efter en ny tilldelning är <code>@count</code> fortfarande 0. <code>||=</code> är egentligen bara en förkortning och kan skrivas om:

{% highlight ruby %}
@count = @count || 32
{% endhighlight %}

<code>||</code>-operatorn i Ruby fungerar så här: först undersöks operanden till vänster. Om den har ett värde som inte är <code>nil</code> eller <code>false</code> så returneras det värdet. Annars returneras värdet på operanden till höger.

Så om vi ska översätta den senaste raden kod till "ren svenska": sätt variabeln <code>@count</code> till 32 om <code>@count</code> inte redan har något värde.

## När har jag användning för detta?

Säg till exempel att du har har en metod i en webbapplikation som hämtar den inloggade användaren från databasen. För att undvika onödiga anrop till databasen väljer du att spara undan användaren i en instansvariabel. Koden för detta ser ut så här:

{% highlight ruby %}
def current_user
  return @current_user unless @current_user.nil?
  @current_user = User.find(session[:user_id])
end
{% endhighlight %}

Det här är ett perfekt användningsområde för ||=-operatorn. Du kan spara en hel rad genom att i stället skriva så här:

{% highlight ruby %}
def current_user
  @current_user ||= User.find(session[:user_id])
end
{% endhighlight %}

_Uppdaterat 2011-11-24: Tack till Linh som fick mig att uppdatera med ett bättre exempel._
