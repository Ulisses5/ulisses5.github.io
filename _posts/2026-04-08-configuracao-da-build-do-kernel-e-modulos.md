---
title: Configurando a build do kernel para módulos
date: 2026-04-08 20:00:00 -0300
categories: [Desenvolvimento de Software Livre, Tutoriais]
tags: [linux,vm,kernel]     # TAG names should always be lowercase
description: O retorno do console do libvirt e o tutorial 3
---

## Introdução

Começo com a jornada para retomar o console e o `ssh` da VM que perdi anteriormente. Depois, faço o **Tutorial 3**, que ensina como fazer um simples módulo para o kernel Linux e como configurar a build para novos recursos do kernel.

Os módulos do kernel são segmentos de código que podem ser dinamicamente inseridos ou removidos do kernel conforme for necessário. Eles alteram as capacidades do kernel sem necessitar de um reboot total do sistema.

## Minha experiência

Seguindo a orientação do monitor e a nossa conclusão que a VM havia sido corrompida de alguma forma, apaguei a imagem do kernel e comecei a reconstruir a VM a partir do **Tutorial 1**. Mesmo com uma imagem recém instalada e sem nenhuma modificação o script `create_vm_virsh` não conseguia subir o console da VM e nem a conexão por `ssh`. 

Como o `launch_vm_qemu` agora estava funcionando, ia tentar seguir utilizando a VM por meio dele. Para permitir a conexão por `ssh`, adicionamos uma flag ao script: `hostfwd=tcp:2222-:22`. Com ela, era possível se conectar a VM pela porta 2222.

Eu ia seguir para o **Tutorial 2** para configurar o `kw` para esse `ssh`, mas antes um amigo compartilhou comigo o que ele fez quando teve um problema parecido com o meu. Uma das mudanças que ele fez foi adicionar a flag `console ttyAMA0` ao final da linha que configurava o boot no script `create_vm_virsh`. Eu vi que essa flag já estava lá e lembrei que um monitor havia colocado ela logo no ínicio dos problemas com o console, numa tentativa de consertar as coisas. Como não estava funcionando, retirei ela e rodei o script novamente. De repente: o console voltou! A conexão por `ssh` funcionava também! 

Fui então refazer o **Tutorial 2**. Chegando no passo que havia dado problema, segui a orientação do meu amigo e fiz a build de todos os módulos do kernel, não apenas o *localmodconfig* que obtemos com o *lsmod* na VM. Após alterar o boot do kernel para a imagem que buildamos na tree do IIO, novamente o console pela `libvirt` não funcionava, mas com o `ssh` ainda ok. Como para meu amigo funcionou, tentei colocar novamente a flag `console ttyAMA0` e assim o `net-dhcp-leases default` voltava a não ter IP nenhum para conexão por `ssh`. Concluí que o problema era essa flag, e, provavelmente, ela foi o motivo do `ssh` ter parado de funcionar sem eu entender o motivo anteriormente. Decidi seguir sem ela e, mesmo sem o console, conseguia usar a VM normalmente conectando por `ssh`.

Agora podia prosseguir finalmente para o **Tutorial 3**. Fiz o módulo de exemplo e configurei sua build tranquilamente. Fiz o *mount* da partição `rootfs` da VM e instalei os módulos, agora incluindo o *simple_mod*. Depois do *unmount*, iniciei a VM pelo `libvirt` novamente e... o console estava lá!! Por algum motivo com essa nova imagem ele carregava sem problemas.

Segui com o tutorial e o concluí sem novas surpresas.

## Conclusão

Depois de muito esforço consegui progredir com os tutoriais. Contando com um pouco de sorte, o console voltou a funcionar inesperadamente. Torço para que a sorte siga ao meu lado!
