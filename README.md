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

![Imagem7](https://github.com/user-attachments/assets/d4a12bda-50a0-40c2-8812-12282f81ab94)

*   **Ethernet Shield W5100**\

![Imagem6](https://github.com/user-attachments/assets/1e93061b-da0a-4760-9a01-4eedb9a66f51)

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

![Imagem2](https://github.com/user-attachments/assets/4ed9e256-7146-4935-ae40-5b524b4c6b76)

***

# 3. Primeiros Testes no Arduino IDE

A interface do **Arduino IDE 2.2.6** foi aberta e explicada:

*   `setup()` → roda uma vez (configuração inicial)
*   `loop()` → roda para sempre (rotina)

![Imagem3](https://github.com/user-attachments/assets/8b7d71d8-23e5-4f5b-9332-5d4565586953)

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

![Imagem4](https://github.com/user-attachments/assets/f8325ad3-2265-4c46-8381-335508b9af9b)

![Imagem5](https://github.com/user-attachments/assets/ab6e7d6d-faa8-4210-a30e-35deb636ec42)

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

![Imagem8](https://github.com/user-attachments/assets/f1e621db-3f49-405f-a12d-12ffaf8d6394)

***

# 6. Código para Obter IP, Máscara, Gateway e DNS

Código executado para mostrar informações de rede:

```cpp
Serial.println(Ethernet.localIP());
Serial.println(Ethernet.subnetMask());
Serial.println(Ethernet.gatewayIP());
Serial.println(Ethernet.dnsServerIP());
```

![Imagem9](https://github.com/user-attachments/assets/8adc083e-50e2-434a-914a-616cd70fde95)

Saída no Serial Monitor:

![Imagem10](https://github.com/user-attachments/assets/d185330a-b12e-497b-a460-0d962bd1bc24)

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

![Imagem11](https://github.com/user-attachments/assets/256d409f-a7b7-473b-96ba-9f61616fe69f)

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

![Imagem12](https://github.com/user-attachments/assets/c2a5a797-70b5-4289-87d3-9a8fd115b758)

HTML inicial (base da página):

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Arduino Web SERVER</title>
</head>
<body>
  <h1>Hello Arduino :)</h1>
</body>
</html>
```

 Explicação (bem direta):

*   `<head>` = bastidores (configurações)
*   `<body>` = palco (o que aparece)
*   `<title>` = nome da aba e pode aparecer em pesquisas

![Imagem13](https://github.com/user-attachments/assets/5f6263f2-2333-4934-bafb-e3a9c89f969a)

Foi adicionado CSS para melhorar aparência:

```css
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

![Imagem14](https://github.com/user-attachments/assets/234939b6-2119-4e01-b20c-2cc9a5b8ae5e)

 Explicações principais:

*   `Ethernet.begin(mac);` → pega IP via DHCP (roteador dá o IP)
*   `server.begin();` → inicia servidor web (porta 80)
*   `Ethernet.localIP()` → mostra o IP do Arduino

***

# 11. Serial Monitor Mostrando IP

![Imagem15](https://github.com/user-attachments/assets/d4c4f125-5565-4a8c-b79f-92ec94e37f50)

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

![Imagem16](https://github.com/user-attachments/assets/0401c6a7-8791-4b5c-9916-0e46823a756c)

 Explicação simples:

*   `PROGMEM` guarda o HTML na memória de programa (flash), poupando RAM.
*   `R"HTML(... )HTML"` permite colar HTML inteiro sem “quebrar” por causa de aspas/símbolos.

***

# 13. Colando o HTML dentro do Arduino

Vocês copiaram o HTML do VS Code e colaram dentro do `R"HTML()HTML"`.

![Imagem17](https://github.com/user-attachments/assets/f36124ca-7769-4da3-96bc-b2de38674559)

***

# 14. Programando o `void loop()` (Servidor HTTP Completo)

![Imagem18](https://github.com/user-attachments/assets/2d40cce9-16ca-4492-b9d6-052d7b9c852d)

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

![Imagem19](https://github.com/user-attachments/assets/fed8516c-f94a-4a1a-9eb0-1efc9a22a056)

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

# **Dia 3 – 16/03/2026**

***

# 16. Objetivo do Dia 3

Controlar um **LED vermelho real** no Arduino pelo **site do Arduino Web Server**, com botões **ON** e **OFF**, funcionando:

*   no computador
*   e principalmente no **celular via Wi‑Fi** (clicando e funcionando)

Início do dia:

*   montagem da rede novamente (roteador, cabos, shield, PC)
*   acesso ao Arduino Web Server (sucesso do dia 2)

![Imagem20](https://github.com/user-attachments/assets/5eb650e8-9e6c-40ed-8ab5-95053fe00e7c)

***

# 17. Definir LED no pino 8 (base)

Foi definido o LED no **pino 8** e testado primeiro como base (simulação).

![Imagem21](https://github.com/user-attachments/assets/2ec8ed39-a3ef-4981-b027-f225f5869dee)

 Conceito rápido:

*   pino digital é um interruptor:
    *   HIGH = liga
    *   LOW = desliga

***

# 18. Documentação do circuito em outro programa (sem simular)

Foi usado o **Fritzing** para documentar o circuito (serve para desenho/documentação, não simula).

![Imagem22](https://github.com/user-attachments/assets/0da4d96a-2203-4a5d-a5c4-22c0fe225c2f)

 Analogia:

*   Tinkercad = simulador
*   Fritzing = “desenho técnico do circuito”

***

# 19. Montagem do circuito no Arduino real

O circuito foi montado fisicamente no Arduino real, replicando o que foi planejado.

![Imagem23](https://github.com/user-attachments/assets/e766c8f5-6106-42f8-933a-eba173461fd2)

 Explicação simples:

*   resistor limita a corrente e protege LED e Arduino\
    (sem resistor, pode queimar).

***

# 20. Alterações no HTML: controle ON/OFF e visual de botões

O HTML no VS Code foi alterado:

*   antes: página simples
*   agora: “Controle do LED” com ON e OFF

![Imagem24](https://github.com/user-attachments/assets/c1f725ac-f562-426f-8cb9-ee96ab91f4a4)

Depois, os links foram estilizados com CSS para parecer **botões** (melhor para toque no celular).

![Imagem25](https://github.com/user-attachments/assets/d58d0b65-6426-466a-a0ed-a45b380e301d)

 Explicação do que mudou (bem direto):

*   Continuam sendo links (`href`), mas com CSS ficam com aparência de botão.
*   ON e OFF ganharam cores diferentes para não confundir.

Exemplo do formato usado (compatível com o código do Arduino do Dia 3):

```html
<h2>Controle do LED</h2>

<a href="/led-on" class="on">ON</a>
<a href="/led-off" class="off">OFF</a>

<style>
  body { font-family: sans-serif; text-align: center; }

  a{
    text-decoration: none;
    font-weight: bold;
    padding: 12px;
    margin: 10px;
    display: inline-block;
    color: #ffffff;
  }
  .on  { background-color: #3498db; }
  .off { background-color: #7f8c8d; }
</style>
```

Analogia:

*   CSS aqui é como “transformar texto clicável em botão de aplicativo”.

***

# 21. Colocar o HTML novo no Arduino Web Server

O código do Arduino Web Server foi reaberto e:

*   HTML antigo removido
*   HTML novo (com botões) colocado no lugar dentro do PROGMEM

![Imagem26](https://github.com/user-attachments/assets/e446dcdf-8161-4443-93e5-64ab6983fb35)

![Imagem27](https://github.com/user-attachments/assets/9e28925f-30f2-4d04-bfb0-29f20eb6282a)

***

# 22. Ajuste do título/identificação

Foi alterado o título para “Arduino IoT”.

![Imagem28](https://github.com/user-attachments/assets/70c7120c-158e-4d3f-aba8-45dcd7cf3e33)

***

# 23. Preparação para interpretar os comandos (alteração no loop)

O trecho antigo que só “lia qualquer coisa” foi apagado para entrar um código capaz de **entender** se foi ON ou OFF.

![Imagem29](https://github.com/user-attachments/assets/64dd4888-7af6-4920-aea9-0c02ba4651e3)

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

![Imagem30](https://github.com/user-attachments/assets/ac72268a-e80c-4cf8-9ec5-79117cf11150)

Lógica do código (explicação simples):

1.  o Arduino lê o texto da requisição (ex.: `GET /led-on ...`)
2.  se encontrar `/led-on` → acende o LED
3.  se encontrar `/led-off` → apaga o LED

Trecho (forma equivalente ao que vocês usaram):

```c++
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

![Imagem31](https://github.com/user-attachments/assets/916ddacc-d101-43d5-8220-1fb9962098cb)

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

Ou seja: foi feito IoT de verdade — interface web → rede → ação física.

***
