---
title: Compilando o kernel com kw
date: 2026-03-31 20:00:00 -0300
categories: [Desenvolvimento de Software Livre, Tutoriais]
tags: [linux,vm,kernel]     # TAG names should always be lowercase
description: A saga do tutorial 2 de DSL
---

## Introdução

Para prosseguir para o desenvolvimento no kernel, o **Tutorial 2** nos guia em como fazer a construção de um kernel Linux para arquitetura ARM64. Isso envolve a compilação do projeto direto da sua fonte, mais específicamente na *tree* do IIO (Industrial I/O system), e a inicialização de fato do sistema dentro da nossa VM, para que possamos testar as mudanças que fizermos nos módulos do kernel.

Esse processo foi facilitado por meio do *kworkflow*, ou simplesmente `kw`, que tem como objetivo auxiliar na configuração do ambiente de desenvolvimento do kernel. Ela é uma ferramenta poderosa que simplifica e automatiza os processos que envolvem a build e deploy de kernels customizados.

## Minha experiência

Esse tutorial acabou virando meu arqui-inimigo da disciplina causando muitos problemas que, até a publicação desse post, ainda não foram completamente resolvidos. 

A instalação e configuração inicial do `kw` não causou problemas. Para fazer a build do kernel customizado, foi necessário conhecer mais do conceito de *cross-compilation*, pois estou desenvolvendo num sistema de arquitetura AMD64 mas como alvo um ARM64. A build demorou bastante, mas não causou nenhuma questão.

Após a build, é necessário mover os módulos compilados para o local correto dentro da VM. Para isso, executei o seguinte comando no host:

```bash
kw deploy --modules
```

O tutorial tem uma observação:

> There may be some errors thrown about `strip` or `make` not being able to do some things, but they may not indicate a real error.

De fato apareceram algumas mensagens de erro para mim, mas apenas segui em frente acreditando estar tudo certo. O próximo passo foi modificar o script de ativação da VM para bootar o kernel agora com a imagem que buildamos da tree do IIO.

Após fazer isso, redefini a VM pela `libvirt` e tentei criá-la novamente:

```bash
sudo virsh shutdown arm64
sudo virsh undefine arm64
create_vm_virsh
```

Contudo, isso resultou na temida tela que indica uma VM inicializada, mas sem console e que não permite a execução de nenhum comando:

![img-description](/images/Console_error.png)
_Ainda tenho pesadelos com essa imagem_

Fui buscar ajuda de um monitor para entender o que estava acontecendo. A primeira abordagem foi reverter as mudanças no script de ativação para usarmos o kernel antigo. O mesmo problema ainda ocorria. Depois, algumas alterações no `kw build --menu` foram feitas pois resolveram essa questão para outro aluno. O problema seguia. Com a VM inicializada pela `libvirt`, ainda era possível se conectar a ela por meio do `ssh` e executar comandos normalmente, e saí da aula acreditando que poderia me virar assim.

Ledo engano.

Na semana seguinte fui tentar fazer a conexão por `ssh` e não consegui. Agora a `libvirt` não indicava mais nenhum IP associado com a VM. Tentamos até mesmo conectar em todos os endereços IP da forma 192.168.122.XXX de 1 a 255, pela possibilidade de algum estar associado a VM e a `libvirt` só não o mostrava, mas sem sucesso. Após mais algumas investigações foi concluído que não tinha mais como conectar-se à VM, mesmo com ela estando inicializada.

A teoria então foi que a VM havia sido corrompida de alguma forma e o que restava seria apagar a imagem do kernel e refazer o *Tutorial 1* com uma nova imagem.

## Conclusão

Essa etapa foi bem frustrante pois nada dava certo e eu não consegui entender o porquê. A esperança é que começando do zero novamente tudo se resolveria (spoiler: não seria bem assim).
