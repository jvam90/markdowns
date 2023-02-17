Webservices são sistemas acessíveis por meio de protocolo HTTP, de forma que eles são auto-contidos, de forma que diferentes aplicações podem acessá-los, podendo ser reutilizados para compor outros sistemas.

Eles possibilitam interoperabilidade entre diferentes sistemas, já que por meio do HTTP, eles podem se comunicar. Basicamente, são pedaços de software disponíveis por meio de algum protocolo, capazes de serem reutilizados.

Antes do REST, essa comunicação e construção de webservices era feita de modo proprietário, ou seja, empresas tinham suas próprias soluções de integração de sistemas.

No começo, era SOAP, o protocolo predominante.

1. Usa http para trocar mensagens de xml
2. Usa Wsdl para comunicação
3. Invoca chamadas por RPC
4. Retorna informações de forma não legível (SOAP envelope)
5. Comunicação pode ser feita por HTTP, SMTP, FTP
6. Arquivos maiores => desempenho ruim
7. O que trafega são os dados mais o envelope (padrão SOAP)

Já o REST é diferente.

1. Estilo arquitetural
2. Usa xml, json, entre outros para trafegar dados
3. Chama os serviços via URL
4. Retorna JSON, XML, etc.
5. Comunicação é feita por HTTP apenas
6. Arquivos menores => desempenho melhor
7. O que trafega são os dados brutos

Mas o que seria REST?

É um estilo de arquitetura de software para sistemas distribuídos. (Representational State Transfer). Ele possui algumas limitações:

1. Cliente separado do servidor
2. Servidor é stateless: não guarda estado do cliente. Cada requisição deve ter os dados suficientes para completá-la.
3. Pode ser cacheable: o cliente deve ser notificado do que se pode guardar em cache
4. Uma interface uniforme entre cliente e servidor:
   1. Identificação dos recursos por URIs
   2. Manipulação dos recursos por suas representações
   3. Mensagens auto descritivas
   4. HATEOAS: hypermedia as the engine of the application state
5. Sistema em camadas: deve ser possível adicionar tecnologias entre o cliente e o servidor de forma transparente para o cliente, como por exemplo: balançeamento de carga, autenticação, etc.
