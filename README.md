## Desafio

## Objetivo
Este desafio faz parte da Pós-Graduação em Go FullCycle, "Labs". O objetivo principal é desenvolver um sistema em Go que receba um CEP, identifica a cidade e retorna o clima atual (temperatura em graus
celsius, fahrenheit e kelvin) juntamente com a cidade e incluir OTEL e zipkin. 

## Serviço 1 (input):

- Receber um input de 8 dígitos via POST, através do schema: `{ "cep": "29902555" }`
- Validar se o input é válido (contém 8 dígitos) e se é uma STRING
- Se válido, encaminhar para o Serviço 2 via HTTP
- Se inválido, retornar:
	- Código HTTP: 422
	- Mensagem: "invalid zipcode"

## Serviço 2 (orquestração):

- Receber um CEP válido de 8 dígitos
- Realizar a pesquisa do CEP e encontrar o nome da localização
- Retornar as temperaturas formatadas em: Celsius, Fahrenheit, Kelvin juntamente com o nome da localização
	- Cenários de resposta:
		- Sucesso:
			- Código HTTP: 200
			- Response Body:
		  ``` json
				  {
					"city": "São Paulo",
					"temp_C": 28.5,
					"temp_F": 28.5,
					"temp_K": 28.5
				  }
		  ```
		- CEP inválido (com formato correto):
			- Código HTTP: 422
			- Mensagem: "invalid zipcode"
		- CEP não encontrado:
			- Código HTTP: 404
			- Mensagem: "can not find zipcode"

### OTEL + Zipkin:

- Implementar tracing distribuído entre Serviço 1 - Serviço 2
- Utilizar span para medir o tempo de resposta do serviço de busca de CEP e busca de temperatura

## Pré-requisitos

- Docker
- Docker Compose

## Executando o projeto

1. Clone o repositório:

2. Configure as variáveis de ambiente:
   Crie um arquivo `.env` no diretório raiz e adicione as seguintes variáveis:
   ```
   API_KEY=weather_api_key
   ```
   Substitua `weather_api_key` por uma chave API válida do API WeatherAPI.

3. Inicie os serviços usando Docker Compose:
   ```
   docker-compose up --build
   ```

4. Os serviços estarão disponíveis nos seguintes endereços:
	- Service1: http://localhost:8080
	- Service2: http://localhost:8081
	- Coletor OpenTelemetry: http://localhost:4318

## Curl

O serviço retornará as informações de temperatura para o CEP fornecido. Use o exemplo abaixo, altere o cep conforme necessário.


```
curl -X POST http://localhost:8080 -H "Content-Type: application/json" -d '{"cep": "29902555"}'
```