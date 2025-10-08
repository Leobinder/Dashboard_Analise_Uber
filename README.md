# 🚖 Dashboard Power BI – Análise de Viagens na UBER

## 📌 Problema de negócio 

A UBER enfrenta desafios operacionais que afetam diretamente a conclusão das viagens e a confiabilidade do serviço. Cancelamentos frequentes e avaliações negativas comprometem a experiência dos usuários e motoristas, impactando a lucratividade da plataforma.

A proposta deste projeto é utilizar dados reais para:

Diagnosticar os principais gargalos operacionais.
Entender os padrões de uso e comportamento dos clientes.
Avaliar a eficiência financeira das viagens.
Propor melhorias estratégicas com base em evidências.



----

## 📌 Objetivos da Análise

```
1. Entender o Público-Alvo e o Propósito da Análise = Apresentar insights relevantes para profissionais de mobilidade urbana, planejamento estratégico e análise de dados.

2. Explorar Fontes de Dados Confiáveis = Utilizar dados públicos da plataforma Kaggle, com linguagem acessível e estrutura clara para públicos técnicos e não técnicos.

3. Analisar Indicadores-Chave de Desempenho (KPIs) = Foco em taxas de cancelamento, desempenho por tipo de veículo, métodos de pagamento e avaliações de motoristas e clientes.

4. Apoiar a Tomada de Decisão Estratégica = Gerar insights que contribuam para ações operacionais e estratégicas voltadas à melhoria da eficiência e da satisfação dos usuários.

``` 
---

## 🧪 Fonte dos Dados

- Plataforma: Kaggle  
- Dataset: [Nome do arquivo ou link]  
- Tipo: Dados públicos de viagens realizadas pela UBER

---

## 🛠 Tecnologias Utilizadas

- Power BI (Modelagem, DAX, Power Query)
- Kaggle (Fonte de dados)
- Visualizações interativas e responsivas

---

## 🧠 Principais Métricas DAX

Este projeto inclui fórmulas DAX avançadas para gerar indicadores relevantes. Abaixo estão as 7 principais métricas utilizadas:

1. **Receita Média por Cliente**
<pre>
Receita Média por Cliente = 
VAR TotalReceita = 
    CALCULATE(
        SUM(Uber[Valor viagem]),
        Uber[STATUS DA VIAGEM] = "completed"
    )
VAR TotalClientes = 
    CALCULATE(
        DISTINCTCOUNT(Uber[Customer ID]),
        Uber[STATUS DA VIAGEM] = "completed"
    )
RETURN 
    DIVIDE(TotalReceita, TotalClientes)
</pre>

2. **Receita Média por Minuto**
<pre>
Receita Média por Minuto =
CALCULATE(
    DIVIDE(
        SUM(Uber[Valor viagem]),
        SUM(Uber[Duração média viagem])
    ),
    Uber[STATUS DA VIAGEM] = "completed"
)
</pre>

3. **Gasto Médio por KM por Corrida**
<pre>
GastoMedioPorKMporCorrida =
AVERAGEX(
    FILTER('Uber', 'Uber'[STATUS DA VIAGEM] = "Completed"),
    DIVIDE('Uber'[Valor viagem], 'Uber'[Distância percorrida], 0)
)
</pre>

4. **Taxa de Cancelamento**
<pre>
Taxa de Cancelamento =
DIVIDE(
    CALCULATE(COUNTROWS(Uber),
    Uber[STATUS DA VIAGEM] IN {"Cancelled by Driver", "Cancelled by Customer"}),
    COUNTROWS(Uber)
)
</pre>

5. **% Receita do Maior Dia**
<pre>
  % Receita do Maior Dia =
VAR ReceitaPorDia =
    ADDCOLUMNS(
        VALUES('uber'[dia da semana]),
        "Receita",
        CALCULATE(
            SUM('Uber'[Valor viagem]),
            'Uber'[STATUS DA VIAGEM] = "Completed"
        )
    )
VAR MaiorReceita =
    MAXX(ReceitaPorDia, [Receita])
VAR ReceitaTotal =
    CALCULATE(
        SUM('Uber'[Valor viagem]),
        'Uber'[STATUS DA VIAGEM] = "completed"
    )
RETURN
    DIVIDE(MaiorReceita, ReceitaTotal)

</pre>

6. **Crescimento Financeiro Mensal**
<pre>
Crescimento Financeiro Mensal =
VAR ReceitaAtual = [Receita Mensal]
VAR ReceitaAnterior =
    CALCULATE(
        [Receita Mensal],
        DATEADD(Uber[DIA], -1, MONTH)
    )
RETURN
    DIVIDE(ReceitaAtual - ReceitaAnterior, ReceitaAnterior, 0)
</pre>

7. **Horário com Maior Receita Média**
<pre>
Horário com Maior Receita Média =
CALCULATE(
    MAX(Uber[Pico Horario]),
    TOPN(
        1,
        ADDCOLUMNS(
            VALUES(Uber[Pico Horario]),
            "ReceitaMedia",
            CALCULATE(
                AVERAGE(Uber[Valor viagem]),
                Uber[STATUS DA VIAGEM] = "completed"
            )
        ),
        [ReceitaMedia], DESC
    )
)
</pre>

> Todos os códigos completos estão disponíveis na pasta `DAX_Metrics`.


---

## 📊 Visualizações

> Aqui você pode adicionar imagens do seu dashboard ou gráficos gerados.

<img width="1206" height="671" alt="Image" src="https://github.com/user-attachments/assets/01060c3b-0bf7-47b4-85a2-4d17ae220ea1" />

<img width="1205" height="679" alt="Image" src="https://github.com/user-attachments/assets/fe388de8-6b33-48b5-8238-2e7bfa2087e3" />

<img width="1204" height="682" alt="Image" src="https://github.com/user-attachments/assets/5da8e190-9f59-4606-bae8-52d9313547fd" />

---
## 🔍 Principais Insights Operacionais e de Experiência


📈 Receita por KM por Mês
A receita por quilômetro apresenta crescimento ao longo dos meses, com picos em períodos de alta demanda. Esse comportamento pode orientar decisões sobre expansão de frota e campanhas promocionais em momentos estratégicos.


📅 Valor Médio por Dia da Semana
Os domingos registram o maior valor médio por viagem, sugerindo alta demanda e menor oferta. Já as segundas-feiras apresentam os menores valores, indicando oportunidade para promoções ou ajustes na precificação.


💳 Formas de Pagamento Mais Utilizadas
O cartão de crédito é o método preferido pelos usuários, seguido por dinheiro e carteira Uber. Isso reforça a importância de otimizar os meios digitais de pagamento para melhorar a experiência e reduzir cancelamentos.


🚦 Distribuição do Status das Viagens
A maioria das viagens é completada com sucesso, mas há um número expressivo de cancelamentos e casos sem motorista disponível. Isso indica a necessidade de melhorar a alocação de veículos e a comunicação com os usuários para reduzir perdas operacionais.


⏱ Tempo Médio de Retirada
O tempo médio de retirada é de 8,46 minutos, com duração média de 29,15 minutos e distância média de 24,64 km. Há espaço para otimizar a logística das viagens e reduzir o tempo de espera dos usuários.


💰 Valor Médio por Viagem
O valor médio por viagem é de R$ 50,83, mas o custo por km (R$ 3,35) supera a receita por minuto (R$ 1,69), indicando a necessidade de revisar a precificação e a eficiência da alocação de veículos.


⭐ Avaliações Médias
As avaliações dos motoristas (4,23) e clientes (4,40) são positivas, demonstrando boa experiência geral. No entanto, os cancelamentos representam 25% das viagens perdidas, exigindo ações corretivas.


🚫 Estratégias para Redução de Cancelamentos
Implementar alertas aos motoristas sobre atrasos, penalidades e bonificações por conclusão de viagens. Melhorar a comunicação com clientes, oferecendo estimativas mais precisas e alternativas rápidas.


🚗 Distribuição Inteligente da Frota
Veículos como Auto e Go Sedan concentram maior volume de viagens em horários de pico. Isso indica a necessidade de redistribuir a frota para atender melhor à demanda em diferentes horários e regiões.

---

## 📌 Conclusões

A análise dos dados de viagens da UBER revelou desafios operacionais significativos, especialmente relacionados à eficiência na conclusão das corridas e à satisfação dos usuários. Os cancelamentos frequentes e as avaliações negativas impactam diretamente a confiabilidade do serviço e a lucratividade da plataforma.
Por meio da modelagem de dados e da criação de métricas personalizadas em DAX, foi possível:

- **`Identificar os principais motivos de cancelamento, tanto por clientes quanto por motoristas.`**

- **`Avaliar o desempenho dos diferentes tipos de veículos, considerando distância média, número de viagens e avaliações.`**

- **`Medir indicadores financeiros como receita por KM, receita por minuto e valor médio por viagem.`**

- **`Medir indicadores financeiros como receita por KM, receita por minuto e valor médio por viagem.`**

- **`Apoiar a tomada de decisões estratégicas, com base em dados reais e confiáveis, visando a melhoria contínua da operação.`**

Este projeto demonstra como a análise de dados pode ser uma ferramenta poderosa para diagnosticar problemas, propor soluções e impulsionar a eficiência e a satisfação em serviços de mobilidade urbana.

--

## 📬 Contato
- Linkedin: [Leonardo Binder](https://www.linkedin.com/in/leonardo-binder/)
- Email: Leonardo.Binder@gmail.com

---

## 🏁 Próximos Passos

- Aplicar modelos preditivos para estimar demanda futura
- Criar alertas automáticos para viagens com alto risco de cancelamento
- Integrar dados em tempo real via API




## 📁 Estrutura do Projeto
