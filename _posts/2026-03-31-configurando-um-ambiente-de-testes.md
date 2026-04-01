---
title: Configurando um ambiente de testes para o kernel Linux
date: 2026-03-31 17:20:00 -0300
categories: [Desenvolvimento de Software Livre, Tutoriais]
tags: [linux,vm,kernel]     # TAG names should always be lowercase
description: Início da jornada em MAC0470
---

## Introdução

A disciplina MAC0470 - Desenvolvimento de Software Livre busca familiarizar os alunos com o ecossistema do SL, desde o entendimento da sua filosofia até a aplicação prática de seus conceitos. O primeiro passo para isso é colaborar com o projeto do kernel Linux.

Para isso, o **Tutorial 1** ensina como construir um ambiente de testes para desenvolvimento no kernel Linux, usando `QEMU` e `libvirt` para auxiliar na configuração de uma máquina virtual (VM) que será usada durante todo o processo de contribuição.

## Minha experiência

Foi um tutorial bem tranquilo, com os passos correndo de maneira fluída. Segui tudo como foi orientado no tutorial, criando uma VM com sistema Debian 12 ARM64, com o detalhe que foi necessário buscar uma imagem mais recente para o download.

Um impeditivo foi que os comandos `virt-filesystems` não eram executados corretamente por falta de permissões, então utilizei `sudo` antes de todos eles. Além disso, o `openssh` não veio instalado por padrão na VM, o que gerou um pouco de confusão mas foi resolvido rapidamente e consegui acessar minha VM por meio do `ssh`. 

Também tentei fazer a seção opcional de definir um diretório compartilhado entre o host e a VM por meio da `libvirt`, mas não foi possível. Segui a orientação do monitor de deixar isso de lado.

## Conclusão

Foi um bom início para essa longa caminhada de tutoriais que seguiremos para poder contribuir de fato para o kernel.
