# ⚡ StreetPower

**Plugin de energia elétrica pública para servidores Rust**

Permite que jogadores comprem energia elétrica temporária diretamente dos postes de luz vanilla do mapa, pagando com Scrap (ou outra moeda configurável). Ideal para servidores que querem adicionar uma camada de economia e utilidade pública ao gameplay elétrico do Rust.

- **Versão:** 1.9.3
- **Autor:** Tropury
- **Compatibilidade:** Oxide / uMod / Carbon
- **Idiomas:** Inglês (en) e Português (pt-BR)

---

## 📋 Sumário

- [Recursos](#-recursos)
- [Como funciona](#-como-funciona)
- [Instalação](#-instalação)
- [Configuração](#-configuração)
- [Comandos](#-comandos)
- [Permissões](#-permissões)
- [Arquivos de dados](#-arquivos-de-dados)
- [Compatibilidade](#-compatibilidade)
- [FAQ](#-faq)
- [Changelog](#-changelog)
- [Licença](#-licença)

---

## 🎯 Recursos

### Para o jogador
- ✅ Compre energia temporária em qualquer poste de luz do mapa
- ✅ Escolha a potência (Watts) e duração (minutos) que desejar
- ✅ Interface CUI elegante com tema industrial do Rust
- ✅ Textos flutuantes 3D acima dos postes mostrando preço/tempo
- ✅ Barra de progresso em tempo real do tempo restante
- ✅ Sistema de equipes — membros do seu time podem usar seu gerador
- ✅ Reembolso automático se o spawn falhar (você nunca perde Scrap)

### Proteção anti-grief
- 🛡️ Gerador **indestrutível** — armas, explosivos, machado e martelo não funcionam
- 🛡️ **Bloqueio de pickup** — ninguém pode pegar o gerador com martelo
- 🛡️ **Bloqueio de roubo de fio** — apenas o dono, admins e membros do time podem conectar/desconectar fios
- 🛡️ Trava de segurança via flag `Locked` + vida altíssima como rede de segurança

### Para o admin
- ⚙️ Configuração granular de preços, limites e cooldowns
- ⚙️ Suporte a qualquer moeda do jogo (Scrap, sulfato, etc.)
- ⚙️ Permissões separadas para admin e uso
- ⚙️ Comandos de gerenciamento completos (reload, info, reset, scan, etc.)
- ⚙️ Persistência completa — sobrevive a restarts do servidor
- ⚙️ Detecção automática de postes em qualquer mapa vanilla
- ⚙️ Comando para cadastrar postes manualmente em mapas customizados

### Robustez técnica
- 🔧 Detecção automática do tipo de gerador (FuelGenerator / ElectricGenerator / IOEntity) — funciona em qualquer versão do Rust
- 🔧 Fallback de prefabs — se um path de prefab mudar em update, tenta alternativos
- 🔧 Reembolso automático em caso de falha de spawn
- 🔧 Balanceamento dinâmico de potência entre as portas conectadas
- 🔧 Logging detalhado para diagnóstico de problemas

---

## 🎮 Como funciona

1. O plugin escaneia o mapa na inicialização e registra todos os postes de luz vanilla (220+ em mapas padrão)
2. Jogadores olham para um poste e pressionam **E** → abre uma interface CUI
3. O jogador escolhe potência (W) e duração (min), vê o preço calculado e clica em COMPRAR
4. O Scrap é descontado, um pequeno gerador é spawnado na base do poste (indestrutível)
5. O jogador conecta um fio do gerador para seus equipamentos
6. A potência é balanceada igualmente entre as portas conectadas a cada 2 segundos
7. Quando o tempo expira, o gerador é removido automaticamente
8. Tudo é persistido — se o servidor reiniciar, o gerador é restaurado com o tempo restante

---

## 📦 Instalação

### Pré-requisitos
- Servidor Rust dedicado com **Oxide/uMod** ou **Carbon** instalado
- Acesso ao FTP ou painel de arquivos do servidor

### Passo a passo

1. **Baixe o arquivo `StreetPower.cs`**
2. **Copie para a pasta de plugins:**
   - Oxide/uMod: `oxide/plugins/`
   - Carbon: `carbon/plugins/`
3. **Reinicie o servidor** OU rode no console:
   ```
   o.reload StreetPower
   ```
4. **Verifique no console** se aparece:
   ```
   [StreetPower] StreetPower: XXX postes detectados via World.Serialization.
   Loaded plugin StreetPower v1.9.3 by Tropury
   ```
5. **Configure o plugin** (opcional) editando:
   ```
   oxide/config/StreetPower.json
   ```
6. **Rode `o.reload StreetPower`** novamente após editar a config

### Primeiro teste

1. Entre no servidor
2. Encontre um poste de luz
3. Mire nele e aperte **E**
4. A interface deve abrir
5. Clique em COMPRAR (com Scrap no inventário)
6. Um gerador deve aparecer na base do poste

---

## ⚙️ Configuração

O arquivo de configuração é criado automaticamente em `oxide/config/StreetPower.json` no primeiro carregamento.

| Parâmetro | Tipo | Padrão | Descrição |
|-----------|------|--------|-----------|
| `PricePerWatt` | float | `2.0` | Preço em moeda por Watt de potência |
| `PricePerMinute` | float | `0.5` | Preço em moeda por minuto de duração |
| `Currency` | string | `"scrap"` | Shortname da moeda (scrap, metal.fragments, etc.) |
| `DefaultPowerOutput` | int | `100` | Potência padrão ao abrir a UI (W) |
| `MinPowerOutput` | int | `10` | Potência mínima permitida (W) |
| `MaxPowerOutput` | int | `300` | Potência máxima permitida (W) |
| `PowerStep` | int | `10` | Incremento dos botões +/− de potência |
| `DefaultDurationMinutes` | int | `60` | Duração padrão ao abrir a UI (min) |
| `MinDurationMinutes` | int | `10` | Duração mínima permitida (min) |
| `MaxDurationMinutes` | int | `120` | Duração máxima permitida (min) |
| `DurationStep` | int | `10` | Incremento dos botões +/− de duração |
| `AllowInfinitePower` | bool | `false` | Se true, energia não expira (duração é ignorada) |
| `CooldownMinutes` | int | `0` | Cooldown entre compras por jogador (0 = sem cooldown) |
| `UsePermissions` | bool | `false` | Se true, requer permissão `streetpower.use` |
| `PermissionName` | string | `"streetpower.use"` | Nome da permissão de uso |
| `ShowChatMessages` | bool | `true` | Exibir mensagens no chat |
| `EnableEffects` | bool | `true` | Efeito visual ao ativar um poste |
| `ShowFloatingLabels` | bool | `true` | Texto 3D flutuante acima dos postes |
| `LabelViewRange` | float | `20.0` | Distância máxima para ver os labels (m) |

### Fórmula de preço

```
Preço = (potência × PricePerWatt) + (duração × PricePerMinute)
```

**Exemplo** com os valores padrão:
- 100W por 60min = (100 × 2.0) + (60 × 0.5) = 200 + 30 = **230 Scrap**

---

## 🎮 Comandos

### Comando de jogador
Nenhum comando de jogador é necessário — tudo é feito pela interface ao pressionar **E** no poste.

### Comandos administrativos (`/streetpower <subcomando>`)

| Comando | Descrição |
|---------|-----------|
| `/streetpower reload` | Recarrega a config e reescaneia postes |
| `/streetpower give` | Ativa energia grátis no poste mais próximo |
| `/streetpower info` | Mostra total de postes registrados e ativos |
| `/streetpower reset` | Desativa todos os postes ativos no servidor |
| `/streetpower nearby` | Lista postes num raio de 50m |
| `/streetpower addpole` | Cadastra manualmente um poste na sua posição |
| `/streetpower removepole` | Remove o poste cadastrado mais próximo (10m) |
| `/streetpower scan` | Escaneia o mapa buscando prefabs de postes |
| `/streetpower scanall [página]` | Scan completo paginado de todos os prefabs elétricos |

---

## 🔐 Permissões

| Permissão | Descrição | Padrão |
|-----------|-----------|--------|
| `streetpower.admin` | Acesso aos comandos administrativos | Concedida manualmente |
| `streetpower.use` | Permite comprar energia (se `UsePermissions` = true) | Todos os jogadores |

### Concedendo permissões

**Oxide/uMod:**
```
oxide.grant group admin streetpower.admin
oxide.grant group default streetpower.use
```

**Carbon:**
```
c.grant group admin streetpower.admin
c.grant group default streetpower.use
```

---

## 💾 Arquivos de dados

O plugin cria dois arquivos de dados em `oxide/data/StreetPower/`:

| Arquivo | Conteúdo |
|---------|----------|
| `data.json` | Postes ativos (comprador, expiração, gerador, potência, duração) + cooldowns |
| `poles.json` | Posições dos postes registrados (auto-detectados + manuais) |

Estes arquivos são salvos automaticamente a cada 5 minutos e no unload do plugin.

---

## 🔧 Compatibilidade

### Plataformas
- ✅ **Oxide/uMod** (testado em versões 2024+)
- ✅ **Carbon** (testado em versões 2024+)

### Versões do Rust
- ✅ Detecta automaticamente o tipo de gerador (FuelGenerator / ElectricGenerator / IOEntity)
- ✅ Funciona em versões antigas e recentes do Rust sem necessidade de atualização
- ✅ Fallback de prefabs para mudanças de path em updates da Facepunch

### Mapas
- ✅ Mapas vanilla procedurais (postes detectados automaticamente)
- ✅ Mapas customizados (use `/streetpower addpole` para cadastrar postes manualmente)
- ✅ Funciona com qualquer mapa que tenha postes de luz padrão

### Conflitos conhecidos
Nenhum conflito conhecido com outros plugins populares. O plugin usa hooks padrão do Oxide e não modifica comportamentos vanilla além do necessário.

---

## ❓ FAQ

**O gerador some quando o servidor reinicia?**
Não. O gerador é restaurado automaticamente com o tempo restante. Você não perde energia.

**Os jogadores podem destruir o gerador de outros?**
Não. O gerador é totalmente indestrutível — armas, explosivos, machado e martelo não funcionam nele.

**Posso usar outra moeda além de Scrap?**
Sim. Basta trocar o `Currency` na config para o shortname do item desejado (ex: `metal.fragments`, `sulfur`, `rope`, etc.).

**O plugin funciona em mapas customizados?**
Sim. Para mapas que usam postes diferentes dos vanilla, use `/streetpower addpole` para cadastrá-los manualmente.

**Como funciona o sistema de equipes?**
Membros do seu time (team) do Rust podem conectar e desconectar fios no seu gerador. Útil para defender monumentos em grupo.

**Tem cooldown entre compras?**
Configurável. Por padrão é 0 (sem cooldown). Defina `CooldownMinutes` na config para ativar.

---

## 📝 Changelog

Veja o arquivo [CHANGELOG.md](CHANGELOG.md) para o histórico completo de versões.

---

## 📄 Licença

Veja o arquivo [LICENSE](LICENSE) para os termos completos da licença comercial.

**Resumo:**
- Licença para **1 servidor** por compra
- Proibido revender, redistribuir ou compartilhar o código
- Pode modificar para uso próprio no servidor licenciado
- Suporte incluído por 90 dias após a compra

---

## 🆘 Suporte

- 📧 **Discord:** [https://discord.gg/dyyVPsAek]

Para reportar bugs ou solicitar features, abra um ticket no Discord com:
1. Versão do Rust do servidor
2. Plataforma (Oxide/Carbon)
3. Log do console no momento do erro
4. Descrição do problema

---

⚡ **StreetPower** — by Tropury · v1.9.3
