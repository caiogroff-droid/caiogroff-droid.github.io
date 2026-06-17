---
title: "Contribuindo para o Linux Kernel — meu primeiro patch"
date: 2026-06-17 00:00:00 -0300
categories: [MAC0470, Patch]
tags: [linux, kernel, iio, patch, open-source]
---

## O começo — um mal entendido

Comecei a trabalhar no patch do Linux no dia 29 de abril. Meu plano inicial era contribuir nos drivers IIO em dupla com outro aluno, mas por um desentendimento da minha parte eu não sabia que estava sem dupla e que deveria estar fazendo um patch diferente.

Depois de conversar com o professor, ficou claro o que precisava ser feito, e na semana seguinte comecei do zero no patch atual.

## O patch em si

O patch que escolhi foi uma refatoração no driver `drivers/iio/light/tsl2591.c`. O driver tinha duas funções, `tsl2591_persist_cycle_to_lit` e `tsl2591_persist_lit_to_cycle`, que eram essencialmente mapeamentos inversos da mesma tabela de 15 entradas — cada uma com um `switch case` enorme repetindo os mesmos pares de valores.

A solução foi simples e limpa: substituí os dois `switch cases` por uma struct de lookup table compartilhada, e as duas funções viraram buscas lineares nessa tabela. Sem duplicação, sem mágica.

```c
struct tsl2591_persist_map {
    u8 cycle;
    int lit;
};

static const struct tsl2591_persist_map tsl2591_persist_table[] = {
    { TSL2591_PRST_ALS_INT_CYCLE_ANY, 1  },
    { TSL2591_PRST_ALS_INT_CYCLE_2,   2  },
    /* ... */
    { TSL2591_PRST_ALS_INT_CYCLE_60,  60 },
};
```

## Enviando o patch

Após uma semana de trabalho, enviei o patch para a lista interna da matéria (CLI), que passou com sucesso sem nenhuma revisão negativa.

Depois disso, enviei o patch de verdade para os mantenedores do subsistema IIO e para a lista `linux-iio@vger.kernel.org`. O envio foi um pouco tardio, então ainda não recebi nenhum retorno dos mantenedores — mas o patch está lá no mundo, e agora é esperar.

Foi uma experiência bem interessante para entender como funciona o fluxo de contribuição para o kernel Linux na prática.