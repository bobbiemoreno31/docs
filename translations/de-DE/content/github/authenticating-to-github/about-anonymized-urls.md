---
title: About anonymized URLs
intro: 'If you upload an image or video to {% data variables.product.product_name %}, the URL of the image or video will be modified so your information is not trackable.'
redirect_from:
  - /articles/why-do-my-images-have-strange-urls/
  - /articles/about-anonymized-image-urls
  - /authenticating-to-github/about-anonymized-image-urls
versions:
  free-pro-team: '*'
topics:
  - Identity
  - Access management
---

Zum Hosten Ihrer Bilder verwendet {% data variables.product.product_name %} den [Open-Source-Projekt-Camo](https://github.com/atmos/camo). Camo generates an anonymous URL proxy for each file which hides your browser details and related information from other users. The URL starts `https://<subdomain>.githubusercontent.com/`, with different subdomains depending on how you uploaded the image.

Videos also get anonymized URLs with the same format as image URLs, but are not processed through Camo. This is because {% data variables.product.prodname_dotcom %} does not support externally hosted videos, so the anonymized URL is a link to the uploaded video hosted by {% data variables.product.prodname_dotcom %}.

Anyone who receives your anonymized URL, directly or indirectly, may view your image or video. To keep sensitive media files private, restrict them to a private network or a server that requires authentication instead of using Camo.

### Probleme mit Camo beheben

In seltenen Fällen erscheinen Bilder, die mit Camo verarbeitet werden, möglicherweise nicht auf {% data variables.product.prodname_dotcom %}. Nachfolgend findest Du einige Schritte, mit denen Du feststellen kannst, wo das Problem liegt.

{% windows %}

{% tip %}

Windows-Benutzer müssen entweder Git Powershell verwenden (neben [{% data variables.product.prodname_desktop %}](https://desktop.github.com/) installiert) oder [curl für Windows](http://curl.haxx.se/download.html) herunterladen.

{% endtip %}

{% endwindows %}

#### Ein Bild wird nicht angezeigt

If an image is showing up in your browser but not on {% data variables.product.prodname_dotcom %}, you can try requesting it locally.

{% data reusables.command_line.open_the_multi_os_terminal %}
1. Fordern Sie die Bildheader mit `curl` an.
  ```shell
  $ curl -I https://www.my-server.com/images/some-image.png
  > HTTP/2 200
  > Date: Fri, 06 Jun 2014 07:27:43 GMT
  > Expires: Sun, 06 Jul 2014 07:27:43 GMT
  > Content-Type: image/x-png
  > Server: Google Frontend
  > Content-Length: 6507
  ```
3. Überprüfe den Wert von `Content-Type`. In diesem Fall ist er `image/x-png`.
4. Überprüfe diesen Inhaltstyp gegen die [Liste der von Camo unterstützten Typen](https://github.com/atmos/camo/blob/master/mime-types.json).

Wenn Dein Inhaltstyp von Camo nicht unterstützt wird, kannst Du mehrere Aktionen versuchen:
  * Wenn Du der Besitzer des Servers bist, der das Bild verwaltet, ändere die Einstellungen so, dass er einen korrekten Inhaltstyp für Bilder zurückgibt.
  * Wenn Du einen externen Dienst zum Verwalten von Bildern verwendest, wende Dich an den Support für diesen Dienst.
  * Stelle einen Pull Request an Camo, um Deinen Inhaltstyp zur Liste hinzuzufügen.

#### Ein kürzlich geändertes Bild wird nicht aktualisiert

Wenn Du ein Bild kürzlich geändert hast und die Änderung in Deinem Browser angezeigt wird, aber nicht auf {% data variables.product.prodname_dotcom %}, kannst Du versuchen, den Zwischenspeicher des Bildes zurückzusetzen.

{% data reusables.command_line.open_the_multi_os_terminal %}
1. Fordern Sie die Bildheader mit `curl` an.
  ```shell
  $ curl -I https://www.my-server.com/images/some-image.png
  > HTTP/2 200
  > Expires: Fri, 01 Jan 1984 00:00:00 GMT
  > Content-Type: image/png
  > Content-Length: 2339
  > Server: Jetty(8.y.z-SNAPSHOT)
  ```

Überprüfe den Wert von `Cache-Control`. In diesem Beispiel gibt es kein `Cache-Control`. Gehe in diesem Fall folgendermaßen vor:
  * Wenn Du Besitzer des Servers bist, der das Bild verwaltet, ändere die Einstellungen so, dass er für Bilder einen `Cache-Control` von `no-cache` zurückgibt.
  * Wenn Du einen externen Dienst zum Verwalten von Bildern verwendest, wende Dich an den Support für diesen Dienst.

 Wenn `Cache-Control` auf `no-cache` gesetzt *ist*, kontaktiere {% data variables.contact.contact_support %} oder durchsuche das {% data variables.contact.community_support_forum %}.

#### Ein Bild aus dem Zwischenspeicher von Camo entfernen

Durch das Bereinigen des Zwischenspeichers wird jeder {% data variables.product.prodname_dotcom %}-Benutzer dazu gezwungen, das Bild erneut anzufordern. Daher solltest Du diesen Vorgang selten und nur dann durchführen, wenn die oben genannten Schritte nicht funktioniert haben.

{% data reusables.command_line.open_the_multi_os_terminal %}
1. Bereinige das Bild, indem Du `curl -X PURGE` auf die Camo-URL anwendest.
  ```shell
  $ curl -X PURGE https://camo.githubusercontent.com/4d04abe0044d94fefcf9af2133223....
  > {"status": "ok", "id": "216-8675309-1008701"}
  ```

#### Bilder in privaten Netzwerken anzeigen

Wenn ein Bild von einem privaten Netzwerk oder von einem Server bereitgestellt wird, der eine Authentifizierung erfordert, kann es nicht von {% data variables.product.prodname_dotcom %} angezeigt werden. Tatsächlich kann es von keinem Benutzer eingesehen werden, ohne dass er dazu aufgefordert wird, sich am Server anzumelden.

Um dieses Problem zu beheben, verschiebe das Bild bitte auf einen öffentlich zugänglichen Dienst.

### Weiterführende Informationen

- „[Proxying user images](https://github.com/blog/1766-proxying-user-images)“ (Proxyvorgang von Benutzerbildern) auf {% data variables.product.prodname_blog %}
