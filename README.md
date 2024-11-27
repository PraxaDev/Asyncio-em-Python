# Asyncio-em-Python
O asyncio trouxe ao Python uma maneira poderosa de lidar com programação assíncrona, otimizando o uso de recursos e melhorando a performance de aplicações que dependem de operações de I/O. Com o crescimento de aplicações web e sistemas baseados em eventos, dominar o asyncio é uma habilidade valiosa para desenvolvedores Python.

## O Que é Programação Assíncrona?
Na programação síncrona, cada tarefa é executada em sequência. Isso significa que o código espera uma tarefa ser concluída antes de passar para a próxima, mesmo que essa tarefa esteja aguardando uma operação de I/O (como uma resposta de rede). Em contraste, a programação assíncrona permite que o programa execute outras tarefas enquanto aguarda, otimizando o uso dos recursos disponíveis.

## O Papel do asyncio
O asyncio é um módulo padrão do Python (disponível a partir da versão 3.4) que permite criar e gerenciar corrotinas, um tipo especial de função que pode ser pausada e retomada. Com ele, você pode construir loops de eventos para lidar com várias tarefas simultaneamente.

## Quando Usar asyncio?
O asyncio é uma escolha ideal para:
- Aplicações web e servidores (como com aiohttp).
- Bots de mensagens e automações.
- Tarefas que envolvem muitas operações de entrada e saída simultaneamente.

Bots de mensagens e automações.

Tarefas que envolvem muitas operações de entrada e saída simultaneamente.

## Conceitos Fundamentos
1. Corrotinas
   - Corrotinas são definidas com a palavra-chave async def e representam funções que podem ser pausadas com await.
```
import asyncio

async def minha_corrotina():
    print("Iniciando a tarefa...")
    await asyncio.sleep(2)  # Simula uma espera assíncrona
    print("Tarefa concluída!")
```
## Executando Múltiplas Corrotinas
O asyncio permite executar várias corrotinas em paralelo, otimizando o tempo de execução.
```
import asyncio

async def tarefa(nome, tempo):
    print(f"Tarefa {nome} iniciada.")
    await asyncio.sleep(tempo)
    print(f"Tarefa {nome} concluída após {tempo} segundos.")

async def main():
    # Executa tarefas simultaneamente
    await asyncio.gather(
        tarefa("A", 2),
        tarefa("B", 3),
        tarefa("C", 1)
    )

asyncio.run(main())
```
Saída esperada:
```
Tarefa A iniciada.
Tarefa B iniciada.
Tarefa C iniciada.
Tarefa C concluída após 1 segundos.
Tarefa A concluída após 2 segundos.
Tarefa B concluída após 3 segundos.
```

## Exemplo Prático: Chamadas a APIs
Imagine que você precisa fazer múltiplas chamadas a uma API. O asyncio pode reduzir significativamente o tempo necessário para completar todas as requisições.
```
import asyncio
import aiohttp  # Biblioteca para requisições assíncronas

async def fetch_url(session, url):
    async with session.get(url) as response:
        print(f"Fetching {url}...")
        return await response.text()

async def main():
    urls = [
        "https://example.com",
        "https://python.org",
        "https://aiohttp.readthedocs.io"
    ]

    async with aiohttp.ClientSession() as session:
        tasks = [fetch_url(session, url) for url in urls]
        responses = await asyncio.gather(*tasks)
        print("Todas as requisições concluídas!")

asyncio.run(main())
```
## Erros Comuns ao Usar asyncio
1. Misturar Código Assíncrono e Síncrono:
    - Evite chamar funções síncronas dentro de corrotinas. Se necessário, use run_in_executor para delegar tarefas síncronas a threads separadas.
2. Esquecer de Usar await:
    - Esquecer await ao chamar uma corrotina resultará em um objeto coroutine não executado.
```
# Errado
async def exemplo_errado():
    resultado = funcao_assincrona()  # Sem await
```
3. Bloqueio de Loop de Eventos:
    - Chamadas bloqueantes, como time.sleep, devem ser evitadas; use asyncio.sleep no lugar.

## Vantagens de asyncio
1. Maior Eficiência:
   - Ideal para aplicações que dependem de operações I/O intensivas, como chamadas de rede ou manipulação de arquivos.
2. Redução de Carga no Sistema:
   - Permite manipular várias conexões simultâneas sem criar threads ou processos extras.
3. Facilidade de Uso:
   - A API do asyncio é bem integrada com outras bibliotecas, tornando o código limpo e legível.
