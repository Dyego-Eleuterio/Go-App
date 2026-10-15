Go Ping API com Docker

Projeto prático desenvolvido durante meus estudos de Docker, com uma aplicação web simples em Go executada em um container.

Sobre o projeto:

A aplicação disponibiliza a rota HTTP /ping, que retorna a resposta pong. O Dockerfile utiliza multi-stage build para compilar a aplicação em uma etapa e copiar o binário gerado para uma imagem final baseada em scratch.

Tecnologias e conceitos
- Go (Golang): implementação do servidor HTTP.
- Docker: empacotamento da aplicação em uma imagem.
- Multi-stage build: separação entre a etapa de compilação e a imagem final.
- Imagem scratch: imagem final mínima, sem uma distribuição Linux completa.
- Porta 8080: porta utilizada pelo servidor da aplicação.

Estrutura do projeto
go-app/
├── Dockerfile
└── main.go

Como funciona:

A aplicação registra a rota /ping. Quando recebe uma requisição nessa rota, responde com o texto pong.

O Dockerfile possui duas etapas:
1. builder: utiliza golang:1.21-alpine, copia o código-fonte para /app e compila o programa com go build.
2. runner: utiliza scratch como imagem final e copia o binário compilado da etapa anterior.
Essa abordagem evita incluir o compilador Go e o código-fonte na imagem final, ajudando a reduzir seu tamanho.

Como executar:

Na pasta go-app, construa a imagem:
docker build -t go-ping .
Execute o container, mapeando a porta 8080 do container para a porta 8080 da máquina:
docker run --rm -p 8080:8080 --name go-ping go-ping
Em outro terminal, teste a rota:
curl http://localhost:8080/ping
Resposta esperada:
pong
Você também pode acessar http://localhost:8080/ping pelo navegador.

O que pratiquei
- Construção de uma imagem Docker a partir de um Dockerfile.
- Compilação de uma aplicação Go dentro de uma etapa de build.
- Uso de multi-stage build.
- Criação de uma imagem final mínima com scratch.
- Exposição e mapeamento da porta usada pela aplicação.
- Teste de uma rota HTTP executada dentro de um container.

Observação técnica:

A imagem scratch não contém um sistema operacional completo nem ferramentas de shell. Por isso, o container executa diretamente o binário compilado. Como a aplicação utiliza apenas recursos da biblioteca padrão do Go, esse formato pode funcionar sem adicionar outras dependências, desde que o binário seja compatível com o ambiente de execução.

Projeto desenvolvido como prática de estudos de Docker