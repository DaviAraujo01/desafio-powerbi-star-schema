# Desafio de Data Modeling: Construção do Modelo Star Schema no Power BI

Este projeto consiste na estruturação de um modelo relacional no formato **Star Schema** utilizando a base de dados *Financial Sample*. O objetivo foi transformar um modelo plano único numa arquitetura otimizada com tabela fato e tabelas dimensão.

---

## 🛠️ Processo de Transformação no Power Query

1. **Base de Origem (`financials_origem`)**: A tabela original foi duplicada para dar origem às dimensões e à tabela fato, garantindo o rastreio da origem dos dados.
2. **Criação da Tabela Fato (`F_Vendas`)**: Mantidos os campos de métricas numéricas e chaves de ligação.
3. **Criação das Tabelas Dimensão**:
   - `D_Produtos`: Métricas agregadas por produto (Média, Mediana, Mínimo e Máximo).
   - `D_Produtos_Detalhes`: Informações de preços e unidades vendidas.
   - `D_Descontos`: Informações referentes às bandas de desconto.
   - `D_Detalhes`: Métricas operacionais como COGS e Vendas Brutas.
4. **Construção da Tabela `D_Calendario`**: Gerada via código DAX para permitir análises temporais:
   ```dax
   D_Calendario = 
   VAR DataMinima = MIN(F_Vendas[Date])
   VAR DataMaxima = MAX(F_Vendas[Date])
   RETURN
   ADDCOLUMNS(
       CALENDAR(DataMinima, DataMaxima),
       "Ano", YEAR([Date]),
       "Número do Mês", MONTH([Date]),
       "Nome do Mês", FORMAT([Date], "mmmm"),
       "Trimestre", "T" & FORMAT([Date], "q")
   )
