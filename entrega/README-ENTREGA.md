# Entrega: Consolidador de reservas de hotel

**Autor:** Marcos Vinicius Moreira
**Ambiente testado:** Windows, Python 3.15.2

## 1. Defeitos corrigidos

| # | Defeito | Teste que o denunciava | Arquivo | Commit |
|---|---|---|---|---|
| 1 | Conversão de câmbio | `test_currency` | `services/currency_converter.py` | `b1ae185` |
| 2 | Cálculo de noites | `test_nights` | `services/stay_calculator.py` | `53ce7ba` |
| 3 | Regra de status (o que é cobrado) | `test_status_rules` | `services/status_policy.py` | `341381a` |
| 4 | Ordenação do ranking de hotéis | `test_top_hotels` | `services/report_builder.py` | `f04f91c` |
| 5 | Leitura truncada do CSV | nenhum (não coberto pelos testes) | `repositories/csv_reservation_repository.py` | `f04f91c` |


## 2. Causa raiz (uma frase por defeito)

1. **Câmbio:** O codigo dividia o amount por rate ao invés de multiplicar, além disso os formatos estavam em float e deveriam estar em decimal.
2. **Noites:** O codigo realizava o calculo apenas com base nos dias como parametro pois, os dois atributos estavam com .day, mas a forma correta e apenas no final do calculo .days. 
3. **Status:** A logica usada permitia a soma de qualquer outro valor que não fosse cancelled o que causava a soma de outros valores além de confirmed.
4. **Ranking:** A função utilizada para elencar os itens da lista era sorted, que ordena os itens por ordem crescente, o oposto do que as regras de negocio impoem. 
5. **Leitura do CSV:** No csv_reservation_repository.py, existia um laço de repetição com um condicional, que limitava, a leitura de dados em 200, quando chegava em 200 linhas lidas, ele aciona um break dentro do if.

## 3. Como investiguei
A forma para investigar e abordar o problema se manteve em todos os testes, entender as regras de negocio, pois assim com a informação do log fica mais facil descobrir onde investiga.
1. **Câmbio:**  Analisando a parte do codigo responsavél pelo conversão de cambio, apos verificação, foi corrigido o calculo matematico,e refatorado pois amount e rate ja são delarados como decimal, então seria errado transformalas em float.
2. **Noites:** verificação de sintaxe correto, par somar datas corretamente
3. **Status:** O resultado das somas no log do teste denunciou o erro de logica.
4. **Ranking:** analisando entendendo as regras de negocio, identificando no codigo qual função era responsavél por definir como os dados seriam mostrados, e apos identifica-la com auxilio de pesquisa e IA descobri que sorted, apresentava os dados da forma inversa do que precisavamos
**Leitura do CSV:** os dados disponibilizados no csv, eram diferentes dos impressos no CLI, encontrando um laço de repetição com uma estrutura condicional de break apos ler 200 linhas, foi necessario apenas a exclusão dessa condicional 


## 4. Como validar
Todos os tests foram repetidos por pytest individualmente, para verificar, validação além de validações manuias como calculos matematicos.
exemplo: -28 no calculo de noites, e impossivél alguem fazer uma reserva negativa, então após correção dados encontrados satisfarotios

```(.venv) PS C:\onfly-desafio> Get-Content output\report.json -Encoding UTF8
{
  "report_version": "1.0",
  "generated_at": "2026-11-15T00:09:58-03:00",
  "summary": {
    "total_reservations": 300,
    "confirmed_reservations": 235,
    "cancelled_reservations": 31,
    "no_show_reservations": 34,
    "total_room_nights": 1347,
    "total_amount_brl": "396170.00",
    "average_nightly_rate_brl": "294.11"
  },
  "by_month": [
    {
      "month": "2024-12",
      "reservations": 35,
      "room_nights": 174,
      "total_brl": "61335.80"
    },
    {
      "month": "2025-01",
      "reservations": 37,
      "room_nights": 235,
      "total_brl": "64662.89"
    },
    {
      "month": "2025-02",
      "reservations": 26,
      "room_nights": 138,
      "total_brl": "45991.09"
    },
    {
      "month": "2025-03",
      "reservations": 39,
      "room_nights": 215,
      "total_brl": "65984.99"
    },
    {
      "month": "2025-04",
      "reservations": 21,
      "room_nights": 146,
      "total_brl": "31335.97"
    },
    {
      "month": "2025-05",
      "reservations": 39,
      "room_nights": 196,
      "total_brl": "63507.98"
    },
    {
      "month": "2025-06",
      "reservations": 38,
      "room_nights": 243,
      "total_brl": "63351.29"
    }
  ],
  "top_hotels": [
    {
      "hotel_id": "H06",
      "hotel_name": "Hotel Salvador Centro",
      "room_nights": 160,
      "total_brl": "45165.46"
    },
    {
      "hotel_id": "H10",
      "hotel_name": "Hotel Madrid, Gran Vía",
      "room_nights": 120,
      "total_brl": "45041.69"
    },
    {
      "hotel_id": "H12",
      "hotel_name": "Hotel Miami Beach",
      "room_nights": 106,
      "total_brl": "41647.69"
    },
    {
      "hotel_id": "H07",
      "hotel_name": "Hotel Brasília Tower",
      "room_nights": 148,
      "total_brl": "39920.52"
    },
    {
      "hotel_id": "H11",
      "hotel_name": "Hotel Buenos Aires",
      "room_nights": 102,
      "total_brl": "37455.69"
    }
  ]
}```



## 5. Resultado do pytest
Exemplo de como era utilizado ```pytest em teste por teste individualmente
(.venv) PS C:\onfly-desafio> pytest tests/test_currency.py -q
.                                                                                                                                                                            [100%]
1 passed in 0.30s```

**Resultados completos**
```(.venv) PS C:\onfly-desafio> pytest -q 
.....                                                                                                                                                                        [100%]
5 passed in 0.69s```
