# Jellyfin local

Setup simples do Jellyfin para uso pessoal, rodando com Docker Compose e usando a imagem oficial.

## Estrutura

- Midia no host: `~/Videos`
- Midia dentro do Jellyfin: `/media`
- Configuracao persistente: `./.data/jellyfin/config`
- Cache persistente: `./.data/jellyfin/cache`
- Acesso web: `http://localhost:8096`
- Acesso pela rede da casa: `http://IP_DO_PC:8096`

## O que este projeto faz

- Sobe um unico container `jellyfin`
- Usa a imagem oficial `jellyfin/jellyfin:10.11.8`
- Mantem configuracao e cache neste repositorio
- Monta `~/Videos` como somente leitura para reduzir risco de alteracao acidental

## Pre-requisitos

- Docker instalado
- Docker Compose instalado
- Pasta `~/Videos` existente com seus arquivos de video

## Subir o Jellyfin

```bash
docker compose up -d
```

## Parar o Jellyfin

```bash
docker compose down
```

## Ver status e logs

```bash
docker compose ps
docker compose logs -f jellyfin
```

## Primeiro acesso

1. Abra `http://localhost:8096` no navegador.
2. Complete o assistente inicial.
3. Ao criar sua biblioteca, use o caminho `/media`.

Se quiser acessar de outro dispositivo da sua rede, abra `http://IP_DO_PC:8096`.

Uma forma simples de descobrir o IP do PC:

```bash
hostname -I
```

## Onde ficam os dados

- `./.data/jellyfin/config`
  Aqui ficam banco, metadados, configuracao e logs do servidor.
- `./.data/jellyfin/cache`
  Aqui fica cache temporario do Jellyfin.
- `~/Videos`
  Aqui ficam seus videos reais. Esta pasta entra no container como somente leitura.

## Backup

Se voce quiser guardar o estado importante do Jellyfin, foque primeiro em:

- `./.data/jellyfin/config`

O `cache` normalmente pode ser recriado.

## Atualizar o Jellyfin

Como a versao esta fixada no `compose.yaml`, a atualizacao fica previsivel:

1. Troque a tag da imagem no `compose.yaml`
2. Rode:

```bash
docker compose pull
docker compose up -d
```

Antes de atualizar, vale fazer backup de `./.data/jellyfin/config`.

## Recursos nao habilitados agora

### Hardware acceleration

O que e:
Usar GPU para ajudar na transcodificacao de video.

Pode ajudar?
Sim, mas principalmente quando o Jellyfin precisa converter video em tempo real para um formato ou qualidade que o dispositivo nao suporta. Isso pesa mais quando ha varios acessos ao mesmo tempo ou quando o PC tem CPU limitada.

Ajuda no seu caso hoje?
Talvez nao. Para uso pessoal simples, principalmente se seus dispositivos ja reproduzirem os arquivos direto, voce pode nao ganhar nada perceptivel agora.

Por que ficou desligado?
Porque aumenta a complexidade do `compose`, depende de hardware compativel e exige mapear dispositivos como `/dev/dri`.

Quando valeria habilitar?
Quando notar travamentos, alto uso de CPU, ou dificuldade para reproduzir alguns formatos em TV/celular.

### DLNA

O que e:
Um jeito de TVs e aparelhos na rede encontrarem o servidor automaticamente sem usar app proprio.

Devemos habilitar agora?
Nao.

Por que nao?
Porque o caminho mais simples e previsivel hoje e usar navegador ou app do Jellyfin. DLNA aumenta a superficie de rede e costuma ser mais util para aparelhos antigos ou limitados.

Pode ajudar no futuro?
Sim, mas so se algum aparelho da sua casa realmente precisar de DLNA para encontrar ou reproduzir a biblioteca.

### Reverse proxy com HTTPS

O que e:
Uma camada na frente do Jellyfin para usar dominio, HTTPS e controle melhor de acesso.

Ajuda agora?
Nao no cenario atual de rede local simples.

Quando valeria habilitar?
Se voce quiser acesso por dominio ou comecar a expor servico fora da sua rede.

### Acesso pela internet

O que e:
Abrir o Jellyfin para acesso fora da sua casa.

Ajuda agora?
Nao. Isso aumenta bastante o cuidado necessario com exposicao, autenticao, HTTPS e manutencao.

Quando valeria habilitar?
Se houver necessidade real de acesso remoto. Quando isso acontecer, o ideal e planejar proxy reverso, HTTPS e revisao de seguranca antes de abrir acesso.

### Escrita na pasta de midia

O que e:
Permitir que o container tenha permissao de escrita em `~/Videos`.

Precisa disso agora?
Nao. Para indexar e reproduzir sua biblioteca, leitura basta.

Por que ficou desligado?
Porque somente leitura e a opcao mais segura e previsivel para um primeiro setup pessoal.

Quando valeria habilitar?
So se surgir uma necessidade concreta que dependa de escrita na propria pasta de midia.

### Atualizacao automatica

O que e:
Deixar o container atualizar sozinho.

Ajuda agora?
Nao necessariamente. Para ambiente pessoal simples, atualizacao manual e mais previsivel.

Quando valeria habilitar?
Se voce aceitar mais automacao em troca de menos controle manual. Mesmo assim, backup antes continua importante.
