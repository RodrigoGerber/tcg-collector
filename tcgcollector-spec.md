# TCG Collector — Especificação Técnica

## 1. Visão geral

O TCG Collector é um aplicativo Android de uso pessoal para catalogar uma coleção de cartas de Trading Card Games, começando por Pokémon TCG.

O foco principal do aplicativo é:

- registrar rapidamente as cartas possuídas;
- organizar as cartas por jogo, coleção, idioma e variante;
- consultar a coleção pessoal;
- criar e gerenciar wishlists;
- pesquisar um catálogo local completo;
- acompanhar preços de referência quando disponíveis;
- evoluir futuramente para identificação de cartas por câmera e visão computacional.

O aplicativo será desenvolvido exclusivamente para Android com Kotlin nativo e Jetpack Compose. Não haverá publicação na Google Play Store, conta de usuário ou backend próprio no MVP.

O nome técnico inicial do projeto será `tcgcollector`. O nome exibido no Android poderá ser alterado posteriormente sem que seja necessário mudar o `applicationId`.

---

## 2. Objetivo

O objetivo do TCG Collector é tornar o cadastro e a consulta de uma coleção pessoal mais rápidos do que o uso de planilhas, sites ou aplicativos genéricos.

O aplicativo deve permitir:

1. Navegar por jogos, séries, coleções e cartas.
2. Manter um catálogo local pesquisável sem internet.
3. Diferenciar impressões em português, inglês e japonês.
4. Diferenciar variantes como normal, holo e reverse holo.
5. Registrar múltiplas cópias equivalentes de forma agrupada.
6. Separar cópias quando possuírem metadados diferentes.
7. Cadastrar várias cartas rapidamente usando um modo checklist.
8. Consultar as cartas possuídas como fluxo principal do aplicativo.
9. Manter uma wishlist detalhada.
10. Exibir preços internacionais de referência quando disponíveis.
11. Exportar e importar todos os dados pessoais em JSON.
12. Servir como base para experimentos futuros com OCR e visão computacional.

---

## 3. Princípios do produto

### 3.1 Coleção pessoal em primeiro lugar

A tela principal deve priorizar as cartas possuídas.

O aplicativo não será orientado principalmente a completar todas as coleções. Progresso de coleção poderá existir como informação complementar, mas não será o principal fluxo de uso.

### 3.2 Cadastro rápido

Cadastrar uma grande quantidade de cartas deve exigir o menor número possível de interações.

O modo checklist por coleção será uma funcionalidade central do MVP.

### 3.3 Dados detalhados, mas opcionais

O usuário poderá registrar diversas informações sobre as cartas, porém os campos adicionais não serão obrigatórios.

O cadastro mínimo será:

```text
Carta + idioma + variante + quantidade
```

Campos opcionais poderão incluir:

```text
Condição
Preço pago
Moeda
Origem
Localização física
Data de aquisição
Observações
```

### 3.4 Local-first e offline-first

O banco local será a fonte de verdade para a interface.

A rede será utilizada para:

- sincronizar o catálogo;
- baixar imagens sob demanda;
- atualizar preços externos;
- futuramente realizar backup em nuvem.

A coleção, wishlist e catálogo já sincronizado devem permanecer acessíveis sem internet.

### 3.5 Modelo preparado para múltiplos TCGs

O MVP será voltado a Pokémon, mas o modelo de domínio não deverá depender exclusivamente de conceitos específicos de Pokémon.

Outros jogos poderão ser adicionados futuramente sem reescrever o núcleo de inventário.

### 3.6 Separação entre catálogo e coleção

Uma carta existente no catálogo não é a mesma coisa que uma cópia física possuída.

O aplicativo deverá separar:

```text
Definição da carta no catálogo
Definição da variante
Grupo de cópias possuídas
Entrada de wishlist
Preço externo
```

### 3.7 Equivalência sem perda de identidade

Cartas em português, inglês e japonês continuarão sendo registros independentes.

Ao mesmo tempo, versões equivalentes poderão ser vinculadas por um agrupamento conceitual para permitir:

- ordenação por Pokémon/personagem;
- navegação independente de idioma;
- comparação entre versões;
- visualização consolidada da coleção;
- busca por metadados gerais;
- associação futura de resultados do scanner.

---

## 4. Escopo do MVP

## 4.1 Funcionalidades incluídas

- Android nativo.
- Kotlin.
- Jetpack Compose.
- Material 3.
- Banco local com Room.
- Preferences DataStore para preferências.
- Catálogo de Pokémon TCG.
- Arquitetura preparada para outros TCGs.
- Sincronização de catálogo via API.
- Catálogo local completamente pesquisável offline.
- Suporte a português, inglês e japonês.
- Séries e coleções.
- Cartas e variantes.
- Tela principal de coleção pessoal.
- Visualização em grid e lista.
- Navegação por coleção.
- Modo checklist por variante.
- Cadastro e edição de quantidade.
- Agrupamento de cópias equivalentes.
- Possibilidade de dividir grupos.
- Metadados opcionais das cópias.
- Localização física em texto livre.
- Wishlist detalhada.
- Busca e filtros.
- Ordenação por metadados gerais.
- Agrupamento conceitual de cartas equivalentes.
- Preço pago manual.
- Estimativa manual.
- Preços internacionais automáticos quando disponíveis.
- Imagens remotas com cache.
- Exportação e importação JSON.
- Estrutura preparada para scanner futuro.

## 4.2 Funcionalidades fora do MVP inicial

- Publicação na Google Play Store.
- iOS.
- Conta de usuário.
- Backend próprio.
- Sincronização entre dispositivos.
- Backup automático no Google Drive.
- Marketplace.
- Compra e venda.
- Preços brasileiros confiáveis e automáticos.
- Extração de preços de páginas sem API oficial.
- OCR em produção.
- Reconhecimento visual em produção.
- Escaneamento contínuo.
- Reconhecimento de páginas inteiras de fichário.
- Classificação automática de condição.
- Detecção automática de falsificações.
- Compartilhamento público de coleção.
- Recursos sociais.

---

## 5. Decisões técnicas

## 5.1 Plataforma

- Android nativo.
- Kotlin.
- Gradle com Kotlin DSL.
- Android Studio.
- Single-activity architecture.

## 5.2 Interface

- Jetpack Compose.
- Material 3.
- Navigation Compose.
- ViewModel.
- StateFlow.
- Unidirectional Data Flow.
- LazyColumn e LazyVerticalGrid.
- Alternância entre visualização em lista e grid.

## 5.3 Persistência

- Room como banco principal.
- Preferences DataStore para preferências simples.
- Migrações explícitas do banco.
- Índices para busca e filtros.
- Room FTS para pesquisa textual, se necessário.

Room será a fonte de verdade para:

- catálogo;
- séries;
- coleções;
- cartas;
- variantes;
- grupos conceituais;
- coleção pessoal;
- wishlist;
- preços;
- metadados de sincronização.

DataStore será utilizado para:

- modo de visualização preferido;
- idioma preferido da interface;
- idiomas visíveis no catálogo;
- filtros persistentes;
- data da última sincronização exibida;
- configurações experimentais do scanner.

## 5.4 Rede

- Retrofit com OkHttp e Kotlin Serialization, ou Ktor Client.
- A primeira implementação pode usar Retrofit por simplicidade e familiaridade do ecossistema Android.
- Respostas remotas serão mapeadas para DTOs.
- DTOs nunca serão usados diretamente pela UI.
- A API externa será abstraída por `CatalogSource`.

## 5.5 Imagens

- Coil para carregamento.
- Cache de memória e disco.
- Imagens carregadas por URL.
- Placeholder para cartas sem imagem.
- Imagens não serão incluídas no backup JSON.

## 5.6 Tarefas em background

- WorkManager para sincronização periódica opcional.
- Sincronização manual disponível.
- Atualização automática poderá ocorrer apenas com rede disponível.
- O MVP não dependerá de execução constante em background.

## 5.7 Injeção de dependências

- Hilt.

O TCG Collector terá mais componentes que o App Blocker:

- múltiplos repositories;
- banco Room;
- API;
- WorkManager;
- scanner futuro;
- providers de preços;
- vários ViewModels.

Por isso, Hilt faz sentido desde o início.

## 5.8 Câmera e machine learning

Fases futuras utilizarão:

- CameraX;
- ML Kit Text Recognition;
- modelos separados para escrita latina e japonesa;
- pipeline de pré-processamento de imagem;
- matching contra o catálogo local;
- interface abstrata `CardRecognizer`;
- posteriormente TensorFlow Lite ou LiteRT para modelos próprios.

---

## 6. Arquitetura

A aplicação seguirá uma arquitetura em camadas:

```text
Compose UI
      ↓
ViewModel
      ↓
Use cases quando necessários
      ↓
Repositories
      ↓
Room / APIs / DataStore / Camera / ML
```

O banco local será a fonte lida pela interface:

```text
API remota
      ↓
CatalogSyncRepository
      ↓
Room
      ↓
Flow
      ↓
ViewModel
      ↓
Compose
```

A interface não deverá observar diretamente respostas da API.

## 6.1 Camadas

### UI

Responsável por:

- renderizar estado;
- receber ações;
- navegação;
- seleção;
- edição;
- feedback de carregamento e erro.

### Domínio

Responsável por:

- agrupamento de cartas;
- regras de criação de grupos de cópias;
- cálculo de inventário;
- combinação de preços;
- filtros complexos;
- importação e exportação;
- matching futuro do scanner.

### Dados

Responsável por:

- Room;
- rede;
- DataStore;
- mapeamentos;
- sincronização;
- cache;
- integração com fontes externas.

## 6.2 Uso de use cases

Use cases serão criados para operações com regra relevante:

```text
AddCardVariantToCollectionUseCase
UpdateOwnedCardQuantityUseCase
SplitOwnedCardGroupUseCase
ToggleChecklistVariantUseCase
ImportCollectionBackupUseCase
ExportCollectionBackupUseCase
SyncCatalogUseCase
ResolveConceptualCardGroupUseCase
MatchOcrResultUseCase
```

Operações CRUD simples poderão chamar repositories diretamente a partir do ViewModel.

---

## 7. Estrutura de módulos

O projeto começará como um modular monolith.

```text
app/
core/
    model/
    database/
    network/
    datastore/
    designsystem/
    common/
    testing/
feature/
    home/
    collection/
    catalog/
    sets/
    checklist/
    carddetails/
    wishlist/
    prices/
    backup/
    settings/
    scanner/
sync/
```

O módulo `scanner` poderá existir inicialmente apenas com contratos, sem implementação completa.

Caso múltiplos módulos Gradle atrasem demais o início do projeto, a mesma estrutura poderá ser mantida como packages dentro de um único módulo `app`.

---

## 8. Estrutura de packages sugerida

```text
com.rodgerber.tcgcollector
├── app/
│   ├── TcgCollectorApplication.kt
│   ├── MainActivity.kt
│   └── TcgCollectorNavHost.kt
│
├── core/
│   ├── model/
│   ├── database/
│   │   ├── entity/
│   │   ├── dao/
│   │   ├── relation/
│   │   ├── migration/
│   │   └── TcgCollectorDatabase.kt
│   ├── network/
│   │   ├── dto/
│   │   ├── api/
│   │   └── mapper/
│   ├── datastore/
│   ├── designsystem/
│   └── common/
│
├── data/
│   ├── catalog/
│   ├── collection/
│   ├── wishlist/
│   ├── pricing/
│   ├── backup/
│   └── sync/
│
├── domain/
│   ├── catalog/
│   ├── collection/
│   ├── wishlist/
│   ├── pricing/
│   ├── backup/
│   └── scanner/
│
├── feature/
│   ├── home/
│   ├── collection/
│   ├── catalog/
│   ├── setdetails/
│   ├── checklist/
│   ├── carddetails/
│   ├── wishlist/
│   ├── backup/
│   ├── settings/
│   └── scanner/
│
└── worker/
    ├── CatalogSyncWorker.kt
    └── PriceSyncWorker.kt
```

---

## 9. Modelo conceitual

A hierarquia principal será:

```text
TcgGame
    └── Series
          └── CardSet
                └── CardPrinting
                      └── CardVariantDefinition
                            └── OwnedCardGroup
```

Paralelamente:

```text
ConceptualCard
    └── múltiplos CardPrinting
```

Exemplo:

```text
ConceptualCard
Pikachu — espécie/personagem geral

CardPrinting A
Pikachu 025/165 — Scarlet & Violet 151 — inglês

CardPrinting B
Pikachu 025/165 — Pokémon 151 — português

CardPrinting C
Pikachu 025/165 — Pokémon Card 151 — japonês
```

Cada impressão continua independente, mas pode ser apresentada em uma visualização consolidada.

---

## 10. Entidades principais

## 10.1 TCG

```kotlin
data class TcgGame(
    val id: String,
    val name: String,
)
```

Exemplo inicial:

```text
id = "pokemon"
name = "Pokémon TCG"
```

## 10.2 Idioma

```kotlin
enum class CardLanguage {
    PORTUGUESE_BRAZIL,
    ENGLISH,
    JAPANESE,
}
```

O modelo poderá futuramente aceitar códigos BCP-47 em vez de depender apenas de enum.

## 10.3 Série

```kotlin
data class CardSeries(
    val id: String,
    val gameId: String,
    val language: CardLanguage,
    val externalSource: String,
    val externalId: String,
    val name: String,
    val releaseOrder: Int?,
)
```

## 10.4 Coleção

```kotlin
data class CardSet(
    val id: String,
    val gameId: String,
    val seriesId: String?,
    val language: CardLanguage,
    val externalSource: String,
    val externalId: String,
    val name: String,
    val printedTotal: Int?,
    val total: Int?,
    val releaseDate: LocalDate?,
    val symbolUrl: String?,
    val logoUrl: String?,
)
```

Coleções em idiomas diferentes serão registros independentes.

Uma tabela opcional de equivalência poderá relacionar coleções correspondentes.

## 10.5 Carta conceitual

Representa metadados mais gerais, independentes da impressão exata.

```kotlin
data class ConceptualCard(
    val id: String,
    val gameId: String,
    val canonicalName: String?,
    val characterKey: String?,
    val nationalDexNumber: Int?,
    val category: String?,
    val primaryType: String?,
)
```

Esse modelo não precisa conter todos os detalhes técnicos do jogo.

Sua principal função será permitir:

- agrupamento;
- ordenação;
- navegação consolidada;
- busca independente de idioma;
- relações entre impressões equivalentes.

O agrupamento poderá começar incompleto e ser enriquecido com o tempo.

## 10.6 Grupo de impressão equivalente

Em alguns casos, uma mesma carta específica é lançada em múltiplos idiomas.

```kotlin
data class PrintingGroup(
    val id: String,
    val gameId: String,
    val conceptualCardId: String?,
    val canonicalSetKey: String?,
    val canonicalCollectorNumber: String?,
    val artworkKey: String?,
)
```

O `PrintingGroup` é mais específico que `ConceptualCard`.

Exemplo:

```text
ConceptualCard:
Pikachu

PrintingGroup:
Pikachu da coleção 151, número 025, mesma arte base

CardPrintings:
- inglês
- português
- japonês
```

## 10.7 Impressão da carta

```kotlin
data class CardPrinting(
    val id: String,
    val gameId: String,
    val setId: String,
    val language: CardLanguage,
    val externalSource: String,
    val externalId: String,
    val printingGroupId: String?,
    val conceptualCardId: String?,
    val localName: String,
    val normalizedName: String,
    val collectorNumber: String,
    val rarity: String?,
    val category: String?,
    val illustrator: String?,
    val imageSmallUrl: String?,
    val imageLargeUrl: String?,
    val releaseDate: LocalDate?,
)
```

Uma impressão representa uma carta identificável de um set e idioma.

## 10.8 Variante disponível

```kotlin
data class CardVariantDefinition(
    val id: String,
    val cardPrintingId: String,
    val variantType: CardVariantType,
    val displayName: String,
    val externalKey: String?,
    val isCatalogConfirmed: Boolean,
)
```

```kotlin
enum class CardVariantType {
    NORMAL,
    HOLO,
    REVERSE_HOLO,
    PROMO,
    FIRST_EDITION,
    UNLIMITED,
    STAMPED,
    COSMOS_HOLO,
    POKE_BALL_REVERSE,
    MASTER_BALL_REVERSE,
    OTHER,
}
```

A lista não deve impedir variantes desconhecidas.

Para isso, a entidade terá também:

```text
displayName
externalKey
customVariantName
```

## 10.9 Grupo de cópias possuídas

```kotlin
data class OwnedCardGroup(
    val id: String,
    val cardPrintingId: String,
    val variantDefinitionId: String,
    val quantity: Int,
    val condition: CardCondition?,
    val purchasePriceMinor: Long?,
    val purchaseCurrency: String?,
    val estimatedPriceMinor: Long?,
    val estimatedCurrency: String?,
    val acquisitionSource: String?,
    val storageLocation: String?,
    val acquiredAt: LocalDate?,
    val notes: String?,
    val createdAt: Instant,
    val updatedAt: Instant,
)
```

Cartas equivalentes poderão ficar agrupadas quando todos os metadados do grupo forem iguais.

Se duas cópias possuírem dados diferentes, elas poderão ser separadas em grupos distintos.

Exemplo:

```text
Pikachu 025/165 — inglês — normal

Grupo A
Quantidade: 2
Condição: NM
Localização: Fichário 151

Grupo B
Quantidade: 1
Condição: LP
Origem: Troca
```

## 10.10 Condição

```kotlin
enum class CardCondition {
    MINT,
    NEAR_MINT,
    LIGHTLY_PLAYED,
    MODERATELY_PLAYED,
    HEAVILY_PLAYED,
    DAMAGED,
}
```

O campo será opcional.

O valor padrão da interface poderá ser `NEAR_MINT`, mas o banco não deve preencher automaticamente sem ação explícita, caso a ausência tenha significado diferente.

## 10.11 Wishlist

```kotlin
data class WishlistEntry(
    val id: String,
    val cardPrintingId: String,
    val desiredVariantDefinitionId: String?,
    val language: CardLanguage,
    val minimumCondition: CardCondition?,
    val priority: WishlistPriority,
    val notes: String?,
    val createdAt: Instant,
    val updatedAt: Instant,
)
```

```kotlin
enum class WishlistPriority {
    LOW,
    MEDIUM,
    HIGH,
    HIGHEST,
}
```

Uma entrada de wishlist poderá existir para uma impressão específica ou, futuramente, para um `PrintingGroup`.

## 10.12 Preço externo

```kotlin
data class CardMarketPrice(
    val id: String,
    val cardPrintingId: String,
    val variantDefinitionId: String?,
    val provider: String,
    val market: String,
    val currency: String,
    val priceType: String,
    val amountMinor: Long,
    val sourceUpdatedAt: Instant?,
    val fetchedAt: Instant,
)
```

Exemplos:

```text
provider = pokemon_tcg_api
market = tcgplayer
currency = USD
priceType = market

provider = pokemon_tcg_api
market = cardmarket
currency = EUR
priceType = trend
```

Preços internacionais serão exibidos como referência, sem afirmar que representam o valor da carta no Brasil.

## 10.13 Metadados de sincronização

```kotlin
data class SyncMetadata(
    val resourceType: String,
    val source: String,
    val language: CardLanguage?,
    val lastAttemptAt: Instant?,
    val lastSuccessAt: Instant?,
    val status: SyncStatus,
    val errorMessage: String?,
)
```

---

## 11. Modelo relacional simplificado

```text
tcg_game
card_series
card_set
conceptual_card
printing_group
card_printing
card_variant_definition
owned_card_group
wishlist_entry
card_market_price
sync_metadata
```

Relações:

```text
tcg_game 1 ── N card_series
tcg_game 1 ── N conceptual_card

card_series 1 ── N card_set
card_set 1 ── N card_printing

conceptual_card 1 ── N printing_group
printing_group 1 ── N card_printing

card_printing 1 ── N card_variant_definition
card_printing 1 ── N owned_card_group
card_printing 1 ── N wishlist_entry
card_printing 1 ── N card_market_price
```

---

## 12. Identificadores

IDs internos não dependerão exclusivamente da API externa.

Cada entidade sincronizada terá:

```text
internalId
externalSource
externalId
```

Exemplo:

```text
internalId = UUID local
externalSource = "tcgdex"
externalId = "sv03.5-025"
```

Isso permite:

- trocar de API;
- combinar fontes;
- corrigir IDs inconsistentes;
- manter referências locais;
- importar dados sem depender da disponibilidade externa.

Uma restrição única deverá existir para:

```text
externalSource + externalId + language
```

quando aplicável.

---

## 13. Fonte do catálogo

A primeira fonte recomendada será TCGdex.

Motivos:

- catálogo multilíngue;
- suporte a português brasileiro;
- suporte a inglês;
- suporte a japonês;
- informações de séries, sets e cartas;
- imagens;
- API REST;
- possibilidade de substituir ou complementar a fonte.

A cobertura varia entre idiomas. O aplicativo deve suportar:

- campos ausentes;
- imagens ausentes;
- coleções incompletas;
- variantes não informadas;
- diferenças entre estruturas por idioma.

A API será encapsulada:

```kotlin
interface CatalogSource {
    suspend fun getSeries(
        gameId: String,
        language: CardLanguage,
    ): List<RemoteSeries>

    suspend fun getSets(
        gameId: String,
        language: CardLanguage,
    ): List<RemoteSet>

    suspend fun getCards(
        setExternalId: String,
        language: CardLanguage,
    ): List<RemoteCard>

    suspend fun getCard(
        externalId: String,
        language: CardLanguage,
    ): RemoteCard
}
```

A implementação inicial:

```text
TcgdexCatalogSource
```

Implementações futuras:

```text
PokemonTcgApiCatalogSource
LocalJsonCatalogSource
ManualCatalogSource
CompositeCatalogSource
```

---

## 14. Sincronização do catálogo

## 14.1 Estratégia

A sincronização seguirá:

```text
Buscar remoto
      ↓
Validar DTO
      ↓
Mapear para modelo local
      ↓
Executar transação Room
      ↓
Atualizar SyncMetadata
      ↓
UI recebe novos dados via Flow
```

## 14.2 Primeira sincronização

A primeira execução poderá:

1. Criar o jogo Pokémon.
2. Perguntar quais idiomas sincronizar.
3. Sincronizar séries e coleções.
4. Permitir sincronização de todas as cartas.
5. Exibir progresso.
6. Permitir uso parcial enquanto a sincronização continua.

Como o objetivo é ter catálogo offline, a configuração padrão será sincronizar os três idiomas.

## 14.3 Atualizações

Tipos possíveis:

- atualização completa manual;
- atualização incremental por coleção;
- atualização automática via WorkManager;
- atualização de uma carta específica;
- correção local manual.

## 14.4 Conflitos

Dados pessoais nunca serão substituídos por sincronização.

Dados do catálogo poderão ser atualizados, mas IDs internos e relações locais serão preservados.

## 14.5 Exclusões remotas

Uma carta removida da API não deve ser apagada imediatamente se:

- estiver na coleção;
- estiver na wishlist;
- tiver preço salvo;
- estiver ligada a uma impressão conceitual.

Ela poderá ser marcada como:

```text
isAvailableFromSource = false
```

---

## 15. Repository pattern

## 15.1 Catálogo

```kotlin
interface CatalogRepository {
    fun observeGames(): Flow<List<TcgGame>>

    fun observeSets(
        gameId: String,
        languages: Set<CardLanguage>,
    ): Flow<List<CardSet>>

    fun observeCards(
        query: CardQuery,
    ): Flow<List<CardPrinting>>

    fun observeCard(
        cardPrintingId: String,
    ): Flow<CardDetails?>

    suspend fun syncCatalog(
        request: CatalogSyncRequest,
    ): SyncResult
}
```

## 15.2 Coleção

```kotlin
interface CollectionRepository {
    fun observeOwnedCards(
        query: OwnedCardQuery,
    ): Flow<List<OwnedCardListItem>>

    fun observeOwnedGroups(
        cardPrintingId: String,
    ): Flow<List<OwnedCardGroup>>

    suspend fun addOrIncrement(
        cardPrintingId: String,
        variantDefinitionId: String,
        defaults: OwnedCardDefaults,
    ): OwnedCardGroup

    suspend fun decrement(
        ownedCardGroupId: String,
    )

    suspend fun updateGroup(
        group: OwnedCardGroup,
    )

    suspend fun splitGroup(
        request: SplitOwnedCardGroupRequest,
    )
}
```

## 15.3 Wishlist

```kotlin
interface WishlistRepository {
    fun observeWishlist(
        query: WishlistQuery,
    ): Flow<List<WishlistListItem>>

    suspend fun add(
        entry: WishlistEntry,
    )

    suspend fun update(
        entry: WishlistEntry,
    )

    suspend fun remove(
        entryId: String,
    )
}
```

## 15.4 Preços

```kotlin
interface PricingRepository {
    fun observePrices(
        cardPrintingId: String,
    ): Flow<List<CardMarketPrice>>

    suspend fun refreshPrices(
        cardPrintingId: String,
    ): PriceRefreshResult
}
```

## 15.5 Backup

```kotlin
interface BackupRepository {
    suspend fun exportBackup(
        destination: Uri,
    ): BackupResult

    suspend fun importBackup(
        source: Uri,
        strategy: ImportStrategy,
    ): ImportResult
}
```

---

## 16. Modo checklist por coleção

O modo checklist será uma das principais formas de cadastro.

## 16.1 Fluxo

```text
Abrir catálogo
      ↓
Selecionar coleção
      ↓
Abrir modo checklist
      ↓
Escolher idioma da impressão
      ↓
Visualizar cartas ordenadas
      ↓
Expandir variantes
      ↓
Usar + e − para alterar quantidades
      ↓
Salvar automaticamente
```

## 16.2 Entrada por variante

Cada variante será uma entrada independente.

Exemplo:

```text
Bulbasaur 001/165

Normal
[−] 2 [+]

Reverse holo
[−] 1 [+]
```

No modo compacto:

```text
Bulbasaur 001/165
✓ Normal ×2
✓ Reverse holo ×1
```

## 16.3 Interações

### Toque no botão `+`

- Se não existir grupo correspondente, criar com quantidade 1.
- Se existir exatamente um grupo compatível, incrementar.
- Se existirem múltiplos grupos com metadados distintos, abrir seletor ou usar o grupo padrão.

### Toque no botão `−`

- Reduzir quantidade do grupo padrão.
- Ao chegar a zero, remover ou confirmar remoção conforme preferência.

### Toque na linha

- Abrir detalhes.
- Permitir editar metadados opcionais.
- Exibir todos os grupos possuídos daquela variante.

### Pressionamento longo

Poderá abrir um menu rápido:

```text
Adicionar com detalhes
Definir quantidade
Adicionar à wishlist
Ver carta
```

## 16.4 Grupo padrão

Para cadastro rápido, o app poderá criar grupos com:

```text
Condição: não informada
Preço: não informado
Origem: não informada
Localização: localização padrão opcional
```

O usuário poderá definir uma localização padrão para a sessão de checklist:

```text
Adicionar tudo desta sessão em:
Fichário 151
```

Isso não será obrigatório no MVP, mas o modelo permitirá.

## 16.5 Variantes desconhecidas

Quando a API não informar as variantes corretamente, o usuário poderá:

- adicionar variante manual;
- usar `OTHER`;
- definir nome customizado;
- reutilizar a variante em outras cartas daquele set, futuramente.

---

## 17. Agrupamento de cópias

A chave de agrupamento padrão será:

```text
cardPrintingId
variantDefinitionId
condition
purchasePrice
purchaseCurrency
acquisitionSource
storageLocation
acquiredAt
notes
```

Se todos os campos forem iguais, cópias podem compartilhar um grupo.

Na prática, o cadastro rápido considerará principalmente:

```text
cardPrintingId + variantDefinitionId
```

e usará um grupo padrão sem metadados adicionais.

Quando o usuário editar uma cópia específica, o app poderá dividir o grupo.

## 17.1 Divisão de grupo

Exemplo:

```text
Antes:
3x Pikachu normal

Ação:
Mover 1 cópia para Fichário 2

Depois:
2x Pikachu normal — sem localização
1x Pikachu normal — Fichário 2
```

## 17.2 União de grupos

Se dois grupos passarem a ter os mesmos atributos, o app poderá oferecer:

```text
Mesclar grupos equivalentes
```

A mesclagem não precisa ser automática no MVP.

---

## 18. Visualizações da coleção

## 18.1 Tela inicial

A Home será orientada à coleção pessoal.

Sugestão:

```text
Minha coleção

Buscar cartas...

[Grid] [Lista]   [Filtros]

Recentemente adicionadas
Coleções
Wishlist
Valor de referência
```

## 18.2 Grid

O grid priorizará:

- imagem;
- nome;
- número;
- coleção;
- idioma;
- quantidade total;
- badges de variantes.

## 18.3 Lista

A lista será mais compacta e adequada para:

- ordenação;
- filtros;
- quantidades;
- edição rápida;
- visualização de idioma e variante.

## 18.4 Visualização consolidada

A visualização consolidada agrupará impressões por metadados gerais.

Possíveis agrupamentos:

```text
Por personagem/Pokémon
Por número da Pokédex
Por grupo de impressão
Por ilustrador
Por tipo
Por coleção conceitual
```

Exemplo:

```text
Pikachu

6 impressões possuídas
3 idiomas
9 cópias

[Ver versões]
```

Ao abrir:

```text
Pikachu — impressão da coleção 151

Português
- Normal ×2
- Reverse ×1

Inglês
- Normal ×1

Japonês
- Master Ball reverse ×1
```

Essa visualização não altera a identidade dos registros.

## 18.5 Ordenações

- nome localizado;
- nome canônico;
- número da coleção;
- número da Pokédex;
- coleção;
- data de lançamento;
- data de adição;
- quantidade;
- raridade;
- valor de referência;
- ilustrador.

---

## 19. Busca

A busca deverá considerar:

- nome localizado;
- nome canônico;
- nome normalizado;
- número da coleção;
- nome da coleção;
- série;
- raridade;
- ilustrador;
- tipo;
- notas pessoais;
- localização física;
- aliases.

## 19.1 Normalização

Para português e inglês:

- lowercase;
- remoção opcional de acentos;
- normalização de espaços;
- normalização de pontuação.

Para japonês:

- preservar texto original;
- permitir aliases romanizados futuramente;
- manter nome canônico quando disponível.

## 19.2 FTS

Room FTS poderá indexar:

```text
localName
normalizedName
canonicalName
collectorNumber
setName
illustrator
aliases
```

Notas pessoais podem usar índice separado.

---

## 20. Filtros

## 20.1 Catálogo

- jogo;
- idioma;
- série;
- coleção;
- raridade;
- tipo;
- variante disponível;
- ilustrador;
- possuída;
- wishlist.

## 20.2 Coleção pessoal

- idioma;
- coleção;
- variante;
- condição;
- localização;
- quantidade;
- data de aquisição;
- com preço;
- sem preço;
- com observações;
- duplicatas.

## 20.3 Wishlist

- prioridade;
- idioma;
- variante;
- condição mínima;
- coleção;
- preço disponível.

---

## 21. Wishlist

## 21.1 Fluxo

```text
Abrir carta
      ↓
Adicionar à wishlist
      ↓
Selecionar variante
      ↓
Selecionar idioma
      ↓
Selecionar condição mínima
      ↓
Selecionar prioridade
      ↓
Adicionar observação
```

## 21.2 Entrada rápida

No modo checklist ou catálogo, será possível adicionar com valores padrão:

```text
Idioma atual
Qualquer variante
Qualquer condição
Prioridade média
```

Depois, os detalhes poderão ser editados.

## 21.3 Relação com a coleção

Quando uma carta da wishlist for adicionada à coleção, o app poderá perguntar:

```text
Remover esta entrada da wishlist?
```

A remoção não deve ser automática sem configuração ou confirmação.

---

## 22. Preços

## 22.1 Objetivo

Os preços servirão como referência, não como avaliação definitiva.

O aplicativo poderá exibir:

```text
Preço pago: R$ 35,00
Estimativa manual: R$ 50,00
TCGplayer market: US$ 12,40
Cardmarket trend: € 10,80
```

## 22.2 Fontes

A arquitetura permitirá múltiplos providers:

```kotlin
interface CardPriceProvider {
    val id: String

    suspend fun getPrices(
        card: CardPrinting,
        variants: List<CardVariantDefinition>,
    ): List<RemoteCardPrice>
}
```

Implementações possíveis:

```text
PokemonTcgApiPriceProvider
TcgplayerPriceProvider
CardmarketPriceProvider
ManualPriceProvider
CompositePriceProvider
```

A primeira integração poderá usar preços retornados pela Pokémon TCG API quando houver correspondência confiável.

## 22.3 Correspondência

Preços só deverão ser associados automaticamente quando houver confiança em:

- coleção;
- número;
- idioma ou mercado;
- variante;
- edição.

Se a correspondência for incerta:

```text
Preço não associado automaticamente
```

ou:

```text
Possível correspondência
```

## 22.4 Moedas

Valores serão armazenados em unidades menores:

```text
USD 12,40 → 1240
EUR 10,80 → 1080
BRL 35,00 → 3500
```

Cada preço preservará sua moeda original.

Conversão cambial poderá ser adicionada futuramente, mas não substituirá o valor original.

## 22.5 Agregação do valor da coleção

O valor total poderá ter diferentes modos:

```text
Somar preço pago
Somar estimativas manuais
Somar referência internacional
```

Não misturar moedas sem conversão explícita.

No MVP, o painel de valor poderá ser simples ou experimental.

## 22.6 Limitações

- preço internacional não representa necessariamente o Brasil;
- cartas em português podem não ter correspondência;
- variantes podem ser classificadas de forma diferente;
- preços podem estar ausentes ou desatualizados;
- condição da carta não será inferida automaticamente.

---

## 23. Imagens

## 23.1 Estratégia

- URL armazenada no catálogo.
- Coil para exibição.
- Cache em disco.
- Placeholder.
- Retry manual.
- Possibilidade futura de salvar imagem local.

## 23.2 Offline

O catálogo textual ficará offline.

Imagens já vistas poderão aparecer pelo cache.

O download completo de imagens de uma coleção poderá ser adicionado futuramente.

---

## 24. Exportação e importação

## 24.1 Formato inicial

JSON versionado.

```json
{
  "schemaVersion": 1,
  "exportedAt": "2026-06-23T12:00:00Z",
  "app": "tcgcollector",
  "ownedCardGroups": [],
  "wishlistEntries": [],
  "customVariants": [],
  "manualPrices": [],
  "preferences": {}
}
```

## 24.2 O que será exportado

- coleção pessoal;
- quantidades;
- metadados opcionais;
- wishlist;
- variantes customizadas;
- preços manuais;
- relações conceituais criadas manualmente;
- configurações relevantes.

## 24.3 O que não precisa ser exportado

- imagens em cache;
- respostas brutas da API;
- catálogo inteiro, desde que possa ser sincronizado novamente;
- preços externos temporários;
- logs.

Para maior segurança, o backup poderá incluir referências suficientes para reconstruir as ligações:

```text
internalId
externalSource
externalId
language
set
collectorNumber
```

## 24.4 Estratégias de importação

```kotlin
enum class ImportStrategy {
    REPLACE_PERSONAL_DATA,
    MERGE,
    PREVIEW_ONLY,
}
```

O fluxo recomendado:

```text
Selecionar arquivo
      ↓
Validar schema
      ↓
Mostrar resumo
      ↓
Escolher merge ou substituição
      ↓
Executar transação
      ↓
Mostrar resultado
```

## 24.5 Google Drive futuro

O backup futuro poderá utilizar:

- Storage Access Framework;
- seleção de pasta do Drive;
- arquivo JSON automático;
- sem necessidade de backend próprio.

---

## 25. Preferências

DataStore poderá guardar:

```kotlin
data class UserPreferences(
    val preferredViewMode: ViewMode,
    val visibleLanguages: Set<CardLanguage>,
    val defaultCatalogLanguage: CardLanguage,
    val defaultWishlistPriority: WishlistPriority,
    val showPrices: Boolean,
    val enableExperimentalScanner: Boolean,
)
```

```kotlin
enum class ViewMode {
    GRID,
    LIST,
}
```

---

## 26. Navegação

Estrutura inicial:

```text
Home
├── Minha coleção
│   ├── Lista/Grid
│   ├── Detalhes da carta
│   └── Editar grupo
│
├── Catálogo
│   ├── Jogos
│   ├── Séries
│   ├── Coleções
│   ├── Checklist
│   └── Detalhes da carta
│
├── Wishlist
│   └── Editar entrada
│
├── Scanner
│   ├── Captura
│   ├── Resultado OCR
│   └── Confirmação
│
└── Configurações
    ├── Sincronização
    ├── Idiomas
    ├── Backup
    └── Experimentos
```

O scanner poderá ficar oculto por feature flag até estar funcional.

---

## 27. Estado da UI

Exemplo da coleção:

```kotlin
data class CollectionUiState(
    val isLoading: Boolean = true,
    val query: String = "",
    val viewMode: ViewMode = ViewMode.GRID,
    val filters: CollectionFilters = CollectionFilters(),
    val sort: CollectionSort = CollectionSort.RECENTLY_ADDED,
    val items: List<OwnedCardListItem> = emptyList(),
    val selectedItemIds: Set<String> = emptySet(),
    val errorMessage: String? = null,
)
```

Checklist:

```kotlin
data class ChecklistUiState(
    val isLoading: Boolean = true,
    val set: CardSet? = null,
    val language: CardLanguage? = null,
    val items: List<ChecklistCardItem> = emptyList(),
    val filter: ChecklistFilter = ChecklistFilter.ALL,
    val pendingChanges: Int = 0,
    val errorMessage: String? = null,
)
```

---

## 28. Sincronização e estado offline

A UI deverá deixar claro:

```text
Catálogo disponível offline
Última sincronização: 23/06/2026
```

Estados possíveis:

```text
Não sincronizado
Sincronizando
Sincronizado
Parcialmente sincronizado
Falha
```

Uma falha de rede não deve apagar dados nem impedir acesso à coleção.

---

## 29. Scanner e visão computacional

O scanner será o principal eixo de evolução após o MVP.

## 29.1 Primeira fase

Objetivo:

```text
Fotografar uma carta
      ↓
Executar OCR
      ↓
Extrair nome e número
      ↓
Buscar candidatos no catálogo local
      ↓
Mostrar top candidatos
      ↓
Usuário confirma
      ↓
Adicionar à coleção
```

## 29.2 Contratos

```kotlin
interface CardRecognizer {
    suspend fun recognize(
        image: CardImage,
        context: RecognitionContext,
    ): CardRecognitionResult
}
```

```kotlin
data class CardRecognitionResult(
    val extractedText: ExtractedCardText,
    val candidates: List<CardCandidate>,
    val debugInfo: RecognitionDebugInfo?,
)
```

```kotlin
data class CardCandidate(
    val cardPrintingId: String,
    val score: Double,
    val matchedFields: Set<MatchedField>,
)
```

## 29.3 Pipeline inicial de OCR

```text
CameraX ImageCapture
      ↓
Correção de rotação
      ↓
Detecção dos limites da carta
      ↓
Recorte
      ↓
Correção de perspectiva
      ↓
Ajuste de contraste
      ↓
OCR latino e/ou japonês
      ↓
Extração de campos
      ↓
Busca no banco
      ↓
Ranking de candidatos
```

## 29.4 Campos úteis

- nome;
- número da coleção;
- total da coleção;
- código da coleção;
- raridade;
- copyright;
- texto de ataques;
- idioma detectado.

O número da carta provavelmente será um dos sinais mais fortes.

Exemplos:

```text
025/165
025/159
sv2a 025/165
```

## 29.5 Ranking

Exemplo de score:

```text
Número exato da carta: peso alto
Coleção compatível: peso alto
Nome aproximado: peso médio
Idioma compatível: peso médio
Raridade: peso baixo
Semelhança visual: peso futuro
```

## 29.6 Scripts de OCR

- modelo latino para português e inglês;
- modelo japonês para cartas japonesas;
- seleção automática ou tentativa em paralelo;
- cache dos modelos necessários.

## 29.7 Dados de treinamento futuros

Para aprender visão computacional, o projeto poderá evoluir para:

- dataset de imagens do catálogo;
- imagens reais fotografadas;
- augmentation;
- classificação por embedding;
- detecção de bordas;
- correção de perspectiva;
- metric learning;
- nearest-neighbor search;
- avaliação top-1 e top-5;
- matriz de confusão;
- comparação OCR versus imagem.

## 29.8 Embeddings futuros

```text
Imagem da câmera
      ↓
Encoder visual
      ↓
Embedding
      ↓
Busca por similaridade
      ↓
Candidatos
```

Embeddings das imagens do catálogo poderão ser pré-calculados.

Como o catálogo é grande, opções futuras incluem:

- índice local reduzido por set;
- SQLite com vetores serializados;
- biblioteca ANN;
- backend experimental;
- geração offline de índice.

## 29.9 Não objetivos iniciais do scanner

- identificar várias cartas simultaneamente;
- estimar condição;
- identificar falsificações;
- reconhecer carta com apenas uma pequena parte visível;
- garantir resultado sem confirmação.

---

## 30. Relação entre OCR e agrupamento conceitual

O agrupamento conceitual ajuda quando o OCR identifica apenas informações gerais.

Exemplo:

```text
OCR encontra:
Pikachu
025
```

O sistema pode:

1. encontrar `ConceptualCard = Pikachu`;
2. localizar `PrintingGroup` com número 025;
3. listar impressões em português, inglês e japonês;
4. usar idioma e imagem para ordenar candidatos;
5. pedir confirmação.

Isso reduz o acoplamento do scanner a uma única língua.

---

## 31. Design system

O MVP usará Material 3, mas com uma camada de design própria.

## 31.1 Componentes

```text
TcgCardThumbnail
TcgCardGridItem
TcgCardListItem
LanguageBadge
VariantBadge
QuantityStepper
PriceLabel
SetHeader
FilterChipGroup
EmptyState
SyncStatusBanner
ChecklistVariantRow
```

## 31.2 Grid

Prioriza imagens e descoberta visual.

## 31.3 Lista

Prioriza densidade e edição rápida.

## 31.4 Temas

- light;
- dark;
- dynamic color opcional;
- cores próprias poderão ser definidas futuramente.

## 31.5 Acessibilidade

- conteúdo descritivo para imagens;
- área mínima de toque;
- suporte a fonte ampliada;
- contraste adequado;
- ações de quantidade com descrição;
- não depender apenas de cor para idioma ou variante.

---

## 32. Performance

## 32.1 Listas

- Paging 3 se o catálogo completo exigir paginação.
- LazyColumn e LazyVerticalGrid.
- IDs estáveis.
- modelos de UI enxutos.
- evitar carregar relações completas em cada item.

## 32.2 Banco

Índices sugeridos:

```text
card_printing(set_id)
card_printing(language)
card_printing(normalized_name)
card_printing(collector_number)
card_printing(conceptual_card_id)
card_printing(printing_group_id)

owned_card_group(card_printing_id)
owned_card_group(variant_definition_id)

wishlist_entry(card_printing_id)

card_market_price(card_printing_id)
```

## 32.3 Imagens

- thumbnails no grid;
- imagem maior apenas em detalhes;
- prefetch limitado;
- cache de disco;
- evitar bitmaps em estado de ViewModel.

## 32.4 Sincronização

- transações por lote;
- upsert;
- evitar limpar e reinserir tudo;
- progresso por idioma e coleção;
- retries controlados.

---

## 33. Tratamento de erros

## 33.1 API indisponível

- manter catálogo local;
- mostrar última sincronização;
- permitir retry;
- não bloquear coleção ou wishlist.

## 33.2 Dados incompletos

- campos nullable;
- placeholder;
- edição manual futura;
- marcar origem e completude.

## 33.3 Imagem ausente

- placeholder;
- retry;
- permitir foto local futuramente.

## 33.4 Preço ausente

```text
Sem preço de referência disponível
```

## 33.5 Backup inválido

- validar versão;
- não alterar banco antes da validação;
- executar importação em transação;
- gerar resumo de erros.

## 33.6 Duplicatas

- identificar pela chave externa;
- detectar grupos equivalentes;
- permitir merge;
- nunca apagar automaticamente dados pessoais.

---

## 34. Logging e diagnóstico

Tags sugeridas:

```text
CatalogSync
TcgdexSource
CatalogMapper
CollectionRepository
Checklist
Wishlist
Pricing
Backup
Scanner
OcrPipeline
CandidateMatcher
```

Registrar:

- início e fim da sincronização;
- quantidade de registros;
- idioma;
- erros de mapeamento;
- campos ausentes;
- importação;
- exportação;
- resultados do OCR em modo debug;
- scores de candidatos;
- tempo de processamento.

Não registrar imagens pessoais sem configuração explícita de debug.

---

## 35. Testes

## 35.1 Testes unitários

### Coleção

- criar grupo;
- incrementar quantidade;
- decrementar;
- remover ao chegar a zero;
- dividir grupo;
- grupos com metadados diferentes;
- merge de grupos;
- quantidade inválida.

### Checklist

- marcar variante inexistente;
- incrementar existente;
- alternar idioma;
- salvar várias alterações;
- variante customizada;
- estado parcial.

### Wishlist

- adicionar;
- atualizar prioridade;
- variante opcional;
- condição mínima;
- remover;
- carta adquirida.

### Preços

- múltiplas moedas;
- preço ausente;
- correspondência exata;
- correspondência ambígua;
- dados antigos;
- agregação sem misturar moedas.

### Agrupamento conceitual

- impressões em três idiomas;
- mesmo Pokémon em artes diferentes;
- mesmo número em sets diferentes;
- ordenação por Pokédex;
- impressão sem grupo.

### Backup

- exportação;
- importação;
- merge;
- substituição;
- IDs remotos ausentes;
- schema antigo;
- rollback por erro.

### Scanner futuro

- parsing de número;
- normalização de nome;
- ranking de candidatos;
- detecção de idioma;
- top-5.

## 35.2 Testes de banco

- migrations;
- constraints;
- cascades;
- índices;
- queries de coleção;
- queries de checklist;
- queries FTS;
- relações.

## 35.3 Testes de UI

- grid/lista;
- filtros;
- adicionar quantidade;
- abrir detalhes;
- estados vazios;
- erro de sincronização;
- importação de backup;
- mudança de tema.

## 35.4 Testes manuais

- primeira sincronização;
- modo avião;
- fechar e reabrir;
- milhares de cartas;
- troca de idioma;
- carta sem imagem;
- set incompleto;
- múltiplas variantes;
- exportar e importar;
- rotação;
- fonte grande;
- tema escuro.

---

## 36. Spike técnico inicial

Antes de construir todas as telas, será criado um protótipo vertical.

## 36.1 Objetivo

Validar o fluxo completo:

```text
TCGdex
      ↓
Retrofit
      ↓
DTO
      ↓
Mapper
      ↓
Room
      ↓
Flow
      ↓
Compose
```

## 36.2 Escopo

- sincronizar uma coleção;
- sincronizar um idioma;
- exibir cartas em grid;
- abrir detalhes;
- criar variantes;
- adicionar uma variante à coleção;
- mostrar coleção pessoal;
- funcionar offline depois da sincronização.

## 36.3 Coleção sugerida para o spike

Pokémon 151 em um idioma inicialmente.

Depois:

- adicionar os três idiomas;
- validar equivalência;
- validar diferenças de catálogo;
- criar modo checklist.

## 36.4 Critério de sucesso

- dados sincronizados;
- imagens exibidas;
- catálogo disponível offline;
- carta adicionada;
- quantidade persistida;
- variantes diferenciadas;
- Room como fonte da UI.

---

## 37. Etapas de implementação

## Fase 1 — Fundação

- criar projeto;
- configurar Compose;
- configurar Hilt;
- configurar Room;
- configurar Retrofit;
- configurar Coil;
- criar navegação;
- criar design system básico.

## Fase 2 — Catálogo remoto e local

- integrar TCGdex;
- criar DTOs;
- criar mappers;
- criar entidades;
- sincronizar jogos, séries e sets;
- sincronizar cartas;
- implementar metadados de sync;
- tratar campos ausentes.

## Fase 3 — Navegação no catálogo

- lista de séries;
- lista de coleções;
- grid/lista de cartas;
- busca;
- filtros básicos;
- detalhes da carta;
- alternância entre idiomas.

## Fase 4 — Coleção pessoal

- criar `OwnedCardGroup`;
- adicionar carta;
- editar quantidade;
- metadados opcionais;
- tela principal;
- grid e lista;
- filtros;
- localização livre.

## Fase 5 — Checklist

- checklist por coleção;
- entrada por variante;
- stepper de quantidade;
- filtro de possuídas;
- filtro de não possuídas;
- cadastro rápido;
- variantes customizadas.

## Fase 6 — Equivalências

- `ConceptualCard`;
- `PrintingGroup`;
- associação inicial;
- visualização consolidada;
- ordenação independente de idioma;
- edição manual de vínculos.

## Fase 7 — Wishlist

- criar entrada;
- editar;
- prioridades;
- condição mínima;
- observações;
- integração com detalhes e checklist.

## Fase 8 — Preços

- preço pago;
- estimativa manual;
- interface `CardPriceProvider`;
- integração internacional inicial;
- exibição de fonte, moeda e data;
- valor agregado experimental.

## Fase 9 — Backup

- exportar JSON;
- importar;
- preview;
- merge;
- replace;
- testes de integridade.

## Fase 10 — Estabilização

- migrations;
- performance;
- tratamento offline;
- erros;
- logs;
- testes;
- polimento visual.

## Fase 11 — Scanner OCR

- CameraX;
- captura individual;
- recorte;
- OCR latino;
- OCR japonês;
- parser;
- matching local;
- confirmação;
- adicionar à coleção.

## Fase 12 — Visão computacional

- coleta de dataset;
- embeddings;
- índice de similaridade;
- combinação OCR + imagem;
- métricas;
- experimentos com modelos locais.

---

## 38. Critérios de conclusão do MVP

O MVP estará concluído quando:

- o catálogo Pokémon puder ser sincronizado;
- português, inglês e japonês forem suportados;
- dados sincronizados puderem ser consultados offline;
- séries e coleções puderem ser navegadas;
- cartas puderem ser exibidas em grid e lista;
- busca funcionar por nome e número;
- uma carta e variante puderem ser adicionadas;
- múltiplas cópias puderem ser agrupadas;
- grupos puderem ser editados;
- metadados opcionais puderem ser registrados;
- a coleção pessoal for a tela principal;
- o checklist permitir cadastrar várias cartas rapidamente;
- variantes forem entradas independentes;
- wishlist puder ser criada e editada;
- visualização consolidada por metadados gerais existir;
- versões em idiomas diferentes permanecerem independentes;
- preços manuais puderem ser registrados;
- referências internacionais puderem ser exibidas quando disponíveis;
- backup JSON puder ser exportado;
- backup puder ser importado;
- falha de rede não impedir acesso à coleção.

O scanner não é necessário para considerar o primeiro MVP concluído, mas sua arquitetura deverá estar preparada.

---

## 39. Riscos conhecidos

### Catálogo multilíngue incompleto

Nem todos os idiomas terão o mesmo nível de cobertura.

Mitigação:

- campos nullable;
- sincronização por idioma;
- correção manual;
- múltiplas fontes futuras;
- não presumir equivalência automática perfeita.

### Variantes inconsistentes

APIs podem não representar todas as variantes comercialmente relevantes.

Mitigação:

- tabela de variantes;
- variantes customizadas;
- edição manual;
- regras específicas por set futuramente.

### Equivalência entre idiomas

Número e nome nem sempre são suficientes.

Mitigação:

- `PrintingGroup`;
- arte;
- set conceitual;
- vínculo manual;
- score de confiança;
- nunca mesclar registros automaticamente sem segurança.

### Preços pouco representativos

Preços dos EUA e Europa não equivalem ao Brasil.

Mitigação:

- exibir fonte e moeda;
- manter preço pago e estimativa manual;
- não chamar referência internacional de valor de mercado brasileiro.

### Volume de dados

Catálogo em múltiplos idiomas pode crescer bastante.

Mitigação:

- Room;
- índices;
- paginação;
- sincronização incremental;
- thumbnails;
- cache.

### OCR variável

Reflexo, sleeve, ângulo e idioma reduzem a precisão.

Mitigação:

- guia de enquadramento;
- correção de perspectiva;
- top-5;
- confirmação manual;
- combinação de sinais.

---

## 40. Nome e identidade do app

Configuração inicial:

```text
Nome do projeto:
tcgcollector

applicationId:
com.rodgerber.tcgcollector

Nome exibido:
TCG Collector
```

O nome exibido poderá ser alterado posteriormente em recursos Android:

```xml
<string name="app_name">Novo nome</string>
```

Alterar o nome exibido não muda a identidade do aplicativo nem o banco local.

O `applicationId` deve ser escolhido com mais cuidado, pois alterá-lo faz o Android tratar o resultado como outro app.

---

## 41. Dependências sugeridas

Versões devem ser escolhidas no momento da implementação.

Categorias:

```text
Jetpack Compose BOM
Material 3
Navigation Compose
Lifecycle ViewModel Compose
Kotlin Coroutines
Room Runtime
Room KTX
Room Compiler via KSP
Hilt Android
Hilt Navigation Compose
Retrofit
OkHttp
Kotlin Serialization
Coil Compose
WorkManager KTX
Paging Compose
CameraX
ML Kit Text Recognition Latin
ML Kit Text Recognition Japanese
JUnit
Turbine
MockK ou fakes manuais
Room Testing
Compose UI Test
```

Não fixar versões no documento evita que a especificação fique rapidamente desatualizada.

---

## 42. Referências técnicas

- Android App Architecture:
  https://developer.android.com/topic/architecture

- Android Offline-first:
  https://developer.android.com/topic/architecture/data-layer/offline-first

- Room:
  https://developer.android.com/training/data-storage/room

- Jetpack Compose lists and grids:
  https://developer.android.com/develop/ui/compose/lists

- TCGdex:
  https://tcgdex.dev/

- TCGdex status:
  https://tcgdex.dev/status

- Pokémon TCG API:
  https://docs.pokemontcg.io/

- ML Kit Text Recognition:
  https://developers.google.com/ml-kit/vision/text-recognition/v2/android

---

## 43. Definição final da primeira versão

```text
Plataforma: Android
Linguagem: Kotlin
UI: Jetpack Compose + Material 3
Banco: Room
Preferências: DataStore
Rede: Retrofit/OkHttp
Imagens: Coil
DI: Hilt
Background: WorkManager
Catálogo inicial: TCGdex
Jogo inicial: Pokémon TCG
Idiomas: português, inglês e japonês
Fonte de verdade: banco local
Visualização: grid e lista
Fluxo principal: coleção pessoal
Cadastro em massa: checklist por variante
Wishlist: detalhada
Preços: manual + referências internacionais
Backup: JSON local
Backend: nenhum
Conta: nenhuma
Scanner inicial futuro: foto + OCR + confirmação
Evolução principal: visão computacional
```

A arquitetura deverá priorizar cadastro rápido, integridade do inventário e evolução gradual do scanner, sem transformar o MVP em uma plataforma excessivamente genérica antes de o fluxo principal estar validado.
