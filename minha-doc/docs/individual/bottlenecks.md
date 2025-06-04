# Caching

## 🧠 Caching com Redis

A utilização de um sistema de **cache em memória** é essencial para aplicações modernas que exigem **alto desempenho** e **baixa latência**. O Redis, sendo um banco de dados in-memory extremamente rápido, é uma das soluções mais adotadas nesse cenário.

---

## 🚀 Vantagens do Cache

* **Baixa latência**: dados são recuperados da memória, muito mais rápido do que consultas ao banco relacional.
* **Redução de carga**: evita sobrecarga em sistemas como PostgreSQL ou serviços externos.
* **Alta disponibilidade**: se bem configurado, pode ser replicado e escalado horizontalmente.
* **Desacoplamento de processamento**: separa dados transitórios e frequentemente acessados da persistência principal.

---


## 🗃️ Estrutura típica no Redis para o Product Service

No contexto do `ProductService`, o Redis é utilizado para armazenar:

### 🔑 Chaves comuns

* **productsAll** → Representa a lista completa de produtos retornada por `findAll()`.
* **products::<id>** → Representa um produto específico por ID, retornado por `findById()`.

### 📦 Exemplo de valores

#### ✅ productsAll

```json
[
  {
    "id": "0195abfb-7074-73a9-9d26-b4b9fbaab0a8",
    "name": "Tomato",
    "price": 10.12,
    "unit": "kg"
  },
  {
    "id": "0195abfe-e416-7052-be3b-27cdaf12a984",
    "name": "Cheese",
    "price": 0.62,
    "unit": "slice"
  }
]
```

#### ✅ products::0195abfb-7074-73a9-9d26-b4b9fbaab0a8

```json
{
  "id": "0195abfb-7074-73a9-9d26-b4b9fbaab0a8",
  "name": "Tomato",
  "price": 10.12,
  "unit": "kg"
}
```

### 🕒 TTL (Time-To-Live)

* Pode ser configurado para **5 minutos**, garantindo que a lista de produtos seja atualizada periodicamente sem sobrecarregar o banco.
* Chaves podem ter TTLs diferentes conforme a frequência de atualização dos dados (ex: `productsAll` com TTL menor que `products::<id>`).

### 🧠 Uso ideal

Esse cache é ideal para otimizar a performance de:

* Interfaces que listam todos os produtos com frequência.
* Consultas individuais por ID (em páginas de detalhe ou edição).

> As chaves são automaticamente **invalidadas ou atualizadas** via anotações `@CacheEvict`, `@CachePut` e `@Cacheable`, conforme definido no seu `ProductService`.

---

## 🪫 Possíveis Bottlenecks com Redis

Apesar do desempenho excelente, o Redis **pode se tornar um gargalo** se mal utilizado:

### 1. **Uso excessivo de cache sem TTL**

Se você armazenar muitos dados sem tempo de expiração (`TTL`), o Redis pode crescer até atingir o limite de memória.

> 💡 *Solução*: aplique TTLs adequados ou políticas de expiração como `volatile-lru`.

### 2. **Cache não invalidado corretamente**

Sem mecanismos de **evict** apropriados, dados desatualizados podem permanecer no cache, causando inconsistência.

> 💡 *Solução*: use `@CacheEvict` ao deletar ou atualizar dados relevantes.

### 3. **Ponto único de falha**

Redis, se utilizado como cache **e** fonte principal de dados (anti-padrão), pode comprometer a resiliência da aplicação.

> 💡 *Solução*: sempre trate falhas de cache como *soft failures* (fallback para o banco).

### 4. **Acesso concorrente não controlado**

Múltiplas requisições podem sobrecarregar o Redis se o acesso for muito frequente e mal dimensionado.

> 💡 *Solução*: analise métricas de cache hit/miss, configure limites de conexão e monitore com Prometheus + Grafana.

---

## 🧪 Exemplo prático com Spring Boot + Redis

### Código do serviço com anotações de cache

```java
package store.product;

import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

import org.springframework.cache.annotation.CacheEvict;
import org.springframework.cache.annotation.CachePut;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.cache.annotation.Caching;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Service;
import org.springframework.web.server.ResponseStatusException;

@Service
public class ProductService {

    private final ProductRepository productRepository;

    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    @Caching(
        put   = { @CachePut(value = "products", key = "#result.id") },
        evict = { @CacheEvict(value = "productsAll", allEntries = true) }
    )
    public Product create(Product product) {
        return productRepository.save(new ProductModel(product)).to();
    }

    @Cacheable(value = "products", key = "#id")
    public Product findById(String id) {
        Optional<ProductModel> optional = productRepository.findById(id);
        if (optional.isEmpty()) {
            throw new ResponseStatusException(HttpStatus.NOT_FOUND, "Product not found");
        }
        return optional.get().to();
    }

    @Caching(
        evict = {
            @CacheEvict(value = "products", key = "#id"),
            @CacheEvict(value = "productsAll", allEntries = true)
        }
    )
    public void delete(String id) {
        if (!productRepository.existsById(id)) {
            throw new ResponseStatusException(HttpStatus.NOT_FOUND, "Product not found");
        }
        productRepository.deleteById(id);
    }

    @Cacheable("productsAll")
    public List<Product> findAll() {
        List<Product> produtos = new ArrayList<>();
        productRepository.findAll().forEach(model -> produtos.add(model.to()));
        return produtos;
    }
}
```

---

### Código da aplicação com cache habilitado

```java
package store.product;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cache.annotation.EnableCaching;   // ⬅ habilita o cache

@SpringBootApplication
@EnableCaching
public class ProductApplication {

    public static void main(String[] args) {
        SpringApplication.run(ProductApplication.class, args);
    }
}
```

---

## 📊 Monitoramento do Cache (Recomendado)

Embora não obrigatório, é **altamente recomendado** monitorar o Redis com:

* **Prometheus Redis Exporter**
* **Grafana Dashboards**
* Métricas como:

  * Taxa de acertos (cache hit rate)
  * Uso de memória
  * Tempo médio de resposta
  * QPS (queries per second)

---
