Configurar Citrix Workspace App


Ficheiro ~/.ICAClient/wfclient.ini

    -KeyboardLayout = (User Profile)
    +KeyboardLayout = British


Erro "You have not chosen to trust "USERTrust RSA Certification Authority", the issuer of the server's security certificate."


Ver detalhe em https://spawn.link/posts/2022-12-11_-_citrix_certs/, mas na essencia:

    ln -s /etc/ssl/certs/USERTrust_RSA_Certification_Authority.pem /opt/Citrix/ICAClient/keystore/cacerts/USERTrust_RSA_Certification_Authority.pem


Usar teclas como ALT+TAB, WindowsKey, etc na sessao:

Edita ficheiro /opt/Citrix/ICAClient/config/All_Regions.ini

    [Virtual Channels\Keyboard]
    TransparentKeyPassthrough=Remote


Tags: citrix,linux
