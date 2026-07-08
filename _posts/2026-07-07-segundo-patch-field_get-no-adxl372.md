---
title: "Segundo patch: FIELD_GET no adxl372"
date: 2026-07-07 00:00:00 -0300
categories: [Linux Kernel, Open Source]
tags: [linux, kernel, iio, drivers, bitfield, patch, open-source, review]
---

## O que foi mudado

No driver `drivers/iio/accel/adxl372.c`, a função `adxl372_set_inactivity_time_ms` fazia a separação manual dos bytes alto e baixo de um valor de 16 bits assim:

```c
reg_val_h = (res >> 8) & 0xFF;
reg_val_l = res & 0xFF;
```

Substituí por máscaras explícitas com `FIELD_GET()`:

```c
#define ADXL372_TIME_INACT_MASK_H  GENMASK(15, 8)
#define ADXL372_TIME_INACT_MASK_L  GENMASK(7, 0)

reg_val_h = FIELD_GET(ADXL372_TIME_INACT_MASK_H, res);
reg_val_l = FIELD_GET(ADXL372_TIME_INACT_MASK_L, res);
```


## A primeira review

Após enviar o patch para a lista `linux-iio@vger.kernel.org`, recebi uma review de um contribuidor chamado Maxwell Doose. Ele deu um `Reviewed-by` — o que é ótimo — mas apontou dois pontos:

1. Estava faltando uma linha em branco após a atribuição do `reg_val_l`.
2. Uma sugestão mais profunda: como o driver já usa `regmap_bulk_write` em outros lugares, por que não refatorar para usar `regmap_bulk_write` com o tipo `__be16` ao invés de dois writes separados?

## O erro que cometi

Ao receber o feedback, achei que deveria corrigir imediatamente e enviei uma `v2` do patch no mesmo dia. O Maxwell então me explicou que:

- Para problemas menores de estilo, não é necessário reenviar — o mantenedor costuma corrigir na hora de aplicar.
- Nova versão só deve ser enviada após pelo menos 24 horas, para dar tempo de mais pessoas revisarem.
- O changelog da nova versão deve ficar abaixo do `---` no patch, para não ser incluído no commit.
- As tags de revisões anteriores devem ser carregadas nas versões novas.

## Próximo passo

A sugestão do `regmap_bulk_write` ainda está em aberto. É uma mudança mais significativa e vai exigir entender melhor como o driver usa bulk writes nos demais lugares antes de propor qualquer alteração.
