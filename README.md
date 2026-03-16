![GitHub License](https://img.shields.io/github/license/lbruss/lab-redes02)

# **Laboratório de IoT com Arduino – Documentação Parcial (Dias 1, 2 e 3 - 11,13,16/03/2026)**

**Integrantes do grupo:**

*   Bruss Loza
*   Daniel
*   Ezequiel

**Professor:**

* José de Assis

***

# **Dia 1 – 11/03/2026**

***

# 1. Objetivo (Parcial)

Iniciar um projeto IoT com Arduino, envolvendo:

*   montagem de rede local
*   configuração de roteador
*   primeiros códigos de comunicação serial
*   utilização do Ethernet Shield
*   obtenção de IP via DHCP
*   testes de comunicação na rede
*   preparação para servidor web

***

# 2. Equipamentos Utilizados

###  Placas e Módulos

*   **Arduino UNO R3**\
    → **Aqui se encaixaria melhor a Imagem 7**

*   **Ethernet Shield W5100**\
    → **Aqui se encaixaria melhor a Imagem 6**

###  Sensores

*   HC-SR04
*   DHT11
*   LDR

###  Acessórios

*   Protoboard
*   LEDs
*   Cabos jumper
*   Cabo USB-B
*   2 Cabos de rede
*   Roteador Wi-Fi

→ **Aqui se encaixaria melhor a Imagem 1 e a Imagem 2**

***

# 3. Primeiros Testes no Arduino IDE

A interface do **Arduino IDE 2.2.6** foi aberta e explicada:

*   `setup()` → roda uma vez (configuração inicial)
*   `loop()` → roda para sempre (rotina)

→ **Aqui se encaixaria melhor a Imagem 3**

Primeiro código (teste de comunicação serial):

```cpp
void setup() {
  Serial.begin(9600);
  Serial.println("Hello World");
}

void loop() {}
```

Explicação simples:

*   `Serial.begin(9600)` abre um “canal de conversa” entre Arduino e computador.
*   `Serial.println()` envia texto para o Serial Monitor e quebra linha.

***

# 4. Teste no Tinkercad e no Arduino Real

O código foi testado:

*   no simulador **Tinkercad**
*   no **Arduino físico**

→ **Aqui se encaixaria melhor a Imagem 4 e a Imagem 5**

Depois, o `println()` foi colocado dentro do `loop()`, fazendo repetir sem parar (aparece várias linhas no Serial Monitor).

 Analogia:

*   `setup()` = “preparar tudo” uma vez
*   `loop()` = “fazer a tarefa repetidamente”

***

# 5. Montagem da Rede IoT

A rede foi montada assim:

*   Arduino + Ethernet Shield → Roteador
*   PC → Roteador
*   Roteador → Internet (rede SENAC pela porta WAN)

→ **Aqui se encaixaria melhor a Imagem 8**

***

# 6. Código para Obter IP, Máscara, Gateway e DNS

Código executado para mostrar informações de rede:

```cpp
Serial.println(Ethernet.localIP());
Serial.println(Ethernet.subnetMask());
Serial.println(Ethernet.gatewayIP());
Serial.println(Ethernet.dnsServerIP());
```

→ **Aqui se encaixaria melhor a Imagem 9**

Saída no Serial Monitor:

→ **Aqui se encaixaria melhor a Imagem 10**

Explicação simples:

*   IP = “número da casa do Arduino na rede”
*   Gateway = “saída do bairro” (roteador)
*   DNS = “lista telefônica” (traduz nomes em IP)
*   Máscara = define “o tamanho do bairro”

***

# 7. Configuração do Roteador + Ping

Foi feito:

*   **Reserva de IP (DHCP Reservation)** para o Arduino (para o IP não mudar)
*   Teste de ping via CMD (PC → Arduino)

→ **Aqui se encaixaria melhor a Imagem 11**

Resultado:

*   sem perda (0%), confirmando comunicação na rede.

***

***

# **Dia 2 – 13/03/2026**

***

# 8. Repetição da Montagem e Configuração

Tudo foi remontado igual ao final do Dia 1:

*   Ethernet Shield conectado
*   Cabos de rede no roteador
*   IP reservado novamente
*   Roteador configurado e Wi‑Fi ativo
*   App **“Ping”** instalado no celular para pingar o Arduino

***

# 9. Desenvolvimento do HTML no VS Code

Configurações feitas:

*   Salvamento automático
*   Extensão **Live Server** instalada
*   Criação/edição de `index.html`

→ **Aqui se encaixaria melhor a Imagem 12**

HTML inicial (base da página):



 Explicação (bem direta):

*   `<head>` = bastidores (configurações)
*   `<body>` = palco (o que aparece)
*   `<title>` = nome da aba e pode aparecer em pesquisas

→ **Aqui se encaixaria melhor a Imagem 13**

Foi adicionado CSS para melhorar aparência:

```
<style>
  body {
    font-family: sans-serif;
    text-align: center;
  }
</style>
```

 Analogia:

*   HTML = “estrutura/esqueleto”
*   CSS = “roupa/estilo”

Anotação correta:

*   VS Code: **Alt + Shift + F** (formata/alinha)
*   Arduino IDE: **Ctrl + T** (formata/alinha)

***

# 10. Programando o Arduino Web Server (Parte 1)

Código inicial no Arduino (servidor na porta 80):

→ **Aqui se encaixaria melhor a Imagem 14**

 Explicações principais:

*   `Ethernet.begin(mac);` → pega IP via DHCP (roteador dá o IP)
*   `server.begin();` → inicia servidor web (porta 80)
*   `Ethernet.localIP()` → mostra o IP do Arduino

***

# 11. Serial Monitor Mostrando IP

→ **Aqui se encaixaria melhor a Imagem 15**

O Arduino exibiu (exemplo):

    Arduino WEB Server
    IP: 192.168.0.101

 Isso confirma:

*   servidor iniciou
*   DHCP funcionou
*   IP válido foi atribuído

***

# 12. Inserindo o HTML dentro do Arduino (PROGMEM)

Criado o “documento HTML” dentro do código usando PROGMEM:

```cpp
const char pagina[] PROGMEM = R"HTML(
HTML AQUI
)HTML";
```

→ **Aqui se encaixaria melhor a Imagem 16**

 Explicação simples:

*   `PROGMEM` guarda o HTML na memória de programa (flash), poupando RAM.
*   `R"HTML(... )HTML"` permite colar HTML inteiro sem “quebrar” por causa de aspas/símbolos.

***

# 13. Colando o HTML dentro do Arduino

Vocês copiaram o HTML do VS Code e colaram dentro do `R"HTML()HTML"`.

→ **Aqui se encaixaria melhor a Imagem 17**

***

# 14. Programando o `void loop()` (Servidor HTTP Completo)

→ **Aqui se encaixaria melhor a Imagem 18**

O servidor passou a:

*   detectar cliente (navegador)
*   enviar cabeçalhos HTTP
*   enviar o HTML armazenado
*   encerrar conexão

 Explicação dos pontos-chave:

*   `server.available()` → detecta navegador (“tocou a campainha”)
*   `HTTP/1.1 200 OK` → “deu certo”
*   `Content-Type: text/html` → “o que vem é HTML”
*   linha em branco → separa cabeçalho do conteúdo (obrigatório)
*   `client.print(FPSTR(pagina));` → envia o HTML do PROGMEM
*   `client.stop();` → fecha conexão

***

# 15. Resultado Final do Dia 2: SUCESSO

→ **Aqui se encaixaria melhor a Imagem 19**

O grupo digitou o IP do Arduino no:

*   computador
*   celular

E a página abriu nos dois dispositivos.

Isso confirma:
 Servidor funcionando\
 HTML funcionando\
 Rede funcionando\
 Arduino como Web Server

***

#  Conclusão Parcial (Dias 1 e 2)

Até aqui o grupo:

*   configurou rede IoT
*   programou comunicação serial
*   montou HTML/CSS
*   inseriu HTML no Arduino
*   criou servidor web
*   acessou a página no navegador (PC e celular)

O Arduino passou a funcionar como um **servidor web** dentro da rede.

***

***

# **Dia 3 – 16/03/2026**

***

# 16. Objetivo do Dia 3

Controlar um **LED vermelho real** no Arduino pelo **site do Arduino Web Server**, com botões **ON** e **OFF**, funcionando:

*   no computador
*   e principalmente no **celular via Wi‑Fi** (clicando e funcionando)

Início do dia:

*   montagem da rede novamente (roteador, cabos, shield, PC)
*   acesso ao Arduino Web Server (sucesso do dia 2)

→ **Aqui se encaixaria melhor a Imagem 20**

Anotação do dia (correta):

*   `...` cria um link clicável (no projeto, vira “botão” de comando).

 Analogia:

*   Link = “mensagem pronta” enviada ao Arduino.

***

# 17. Definir LED no pino 8 (base)

Foi definido o LED no **pino 8** e testado primeiro como base (simulação).

→ **Aqui se encaixaria melhor a Imagem 21**

 Conceito rápido:

*   pino digital é um interruptor:
    *   HIGH = liga
    *   LOW = desliga

***

# 18. Documentação do circuito em outro programa (sem simular)

Foi usado o **Fritzing** para documentar o circuito (serve para desenho/documentação, não simula).

→ **Aqui se encaixaria melhor a Imagem 22**

 Analogia:

*   Tinkercad = simulador
*   Fritzing = “desenho técnico do circuito”

***

# 19. Montagem do circuito no Arduino real

O circuito foi montado fisicamente no Arduino real, replicando o que foi planejado.

→ **Aqui se encaixaria melhor a Imagem 23**

 Explicação simples:

*   resistor limita a corrente e protege LED e Arduino\
    (sem resistor, pode queimar).

***

# 20. Alterações no HTML: controle ON/OFF e visual de botões

O HTML no VS Code foi alterado:

*   antes: página simples
*   agora: “Controle do LED” com ON e OFF

→ **Aqui se encaixaria melhor a Imagem 24**

Depois, os links foram estilizados com CSS para parecer **botões** (melhor para toque no celular).

→ **Aqui se encaixaria melhor a Imagem 25**

 Explicação do que mudou (bem direto):

*   Continuam sendo links (`href`), mas com CSS ficam com aparência de botão.
*   ON e OFF ganharam cores diferentes para não confundir.

Exemplo do formato usado (compatível com o código do Arduino do Dia 3):



Analogia:

*   CSS aqui é como “transformar texto clicável em botão de aplicativo”.

***

# 21. Colocar o HTML novo no Arduino Web Server

O código do Arduino Web Server foi reaberto e:

*   HTML antigo removido
*   HTML novo (com botões) colocado no lugar dentro do PROGMEM

→ **Aqui se encaixaria melhor a Imagem 26**\
→ **Aqui se encaixaria melhor a Imagem 27**

***

# 22. Ajuste do título/identificação

Foi alterado o título para “Arduino IoT”.

→ **Aqui se encaixaria melhor a Imagem 28**

***

# 23. Preparação para interpretar os comandos (alteração no loop)

O trecho antigo que só “lia qualquer coisa” foi apagado para entrar um código capaz de **entender** se foi ON ou OFF.

→ **Aqui se encaixaria melhor a Imagem 29**

Analogia:

*   Antes: o Arduino só entregava a página (sem entender o pedido).
*   Agora: o Arduino precisa “ler a frase” do navegador e decidir a ação.

***

# 24. Código novo no `loop()` para ligar/desligar LED via HTTP

Foi adicionado no `setup()`:

```cpp
pinMode(led, OUTPUT);
```

 Explicação verdadeira e direta:

*   isso configura o pino como **saída**, permitindo controlar o LED corretamente.
*   **não é isso sozinho que “aumenta brilho”**; brilho depende de resistor/corrente/LED, mas `OUTPUT` é obrigatório para funcionar certo.

Depois, foi colocado o novo código no lugar do trecho apagado:

→ **Aqui se encaixaria melhor a Imagem 30**

Lógica do código (explicação simples):

1.  o Arduino lê o texto da requisição (ex.: `GET /led-on ...`)
2.  se encontrar `/led-on` → acende o LED
3.  se encontrar `/led-off` → apaga o LED

Trecho (forma equivalente ao que vocês usaram):

```cpp
String request = "";

while (client.available()) {
  char c = client.read();
  request += c;
}

if (request.indexOf("GET /led-on") >= 0) {
  digitalWrite(led, HIGH);
}

if (request.indexOf("GET /led-off") >= 0) {
  digitalWrite(led, LOW);
}
```

 Analogia:

*   o Arduino “lê a frase” do navegador e procura uma palavra-chave:
    *   achou “led-on” → liga
    *   achou “led-off” → desliga

Depois disso, vocês enviaram o código para o Arduino (upload).

***

# 25. Resultado Final do Dia 3: SUCESSO (controle pelo celular)

→ **Aqui se encaixaria melhor a Imagem 31**

Objetivo atingido:

*   clicou em **ON** → LED acendeu
*   clicou em **OFF** → LED apagou

E o mais importante:
 funcionou no **celular** conectado via Wi‑Fi, apenas clicando.

***

#  Conclusão Parcial (Dias 1, 2 e 3)

Até agora o grupo:

*   montou e validou a rede local (roteador, LAN, WAN SENAC)
*   garantiu estabilidade com **reserva de IP**
*   transformou o Arduino em **Web Server**
*   criou página em **HTML/CSS**
*   acessou a página no PC e no celular
*   e finalmente controlou um **LED real** via navegador (ON/OFF)

Ou seja: vocês fizeram IoT de verdade — interface web → rede → ação física.

***

