# Configurações do NixOS e Linux em geral.
Configurações linux e mais com o NixOS.

---

## Instalação do NixOS.

*Para aprender a dominar o NixOS, siga este roteiro prático. Ele cobre desde a alteração de uma configuração simples até a atualização de versão do sistema.*

**1. Primeira Atualização Geral**
*Mesmo que a ISO seja recente, os pacotes mudam rápido. Sincronize o sistema:*
```bash
sudo nix-channel --update
sudo nixos-rebuild switch --upgrade
```

**Passo 2:** O Fluxo de Trabalho Básico (Configurar):
*No NixOS, você não instala coisas "soltas". Você as descreve no arquivo central.*

Abra o arquivo: sudo nano /etc/nixos/configuration.nix e copie esse aqui:
*Obs.: Esse é minha configuração!!!*
```bash                                                        
 
```

Sincronize: 
```bash
sudo nixos-rebuild switch
```

**Passo 3:** Manutenção Semanal (Atualizar):
*Faça isso uma vez por semana para manter o sistema seguro e em dia.*

Sincronize e atualize tudo: 
```bash
sudo nixos-rebuild switch --upgrade
```

*O que isso faz: Baixa as definições novas do canal e já aplica no seu sistema.*

**Passo 4:** Instalar Programas Temporários ou Permanentes:
*Uma das melhores funções do NixOS para quem está aprendendo.*

Quer usar o python3 ou o htop só agora?:
```bash
nix-shell -p python3
```

*Saia da sessão, o programa não ocupa mais espaço no seu sistema "real".*

environment.systemPackages = with pkgs; [
  git
  vlc
  vscode
  # Adicione o que quiser aqui
];

**Passo 5:** Limpeza de Disco (Faxina):
*Como o NixOS guarda versões antigas (gerações), o disco pode encher.*
*Caso não tenha colocado na config.nix*

Remova o que é velho: 
```bash
sudo nix-collect-garbage -d
```

*Dica: Só faça isso se o sistema atual estiver funcionando perfeitamente, pois isso apaga os pontos de restauração antigos.*

**Passo 6:** Troca de Versão (Upgrade de 6 meses)
*Quando sair uma versão nova (ex: mudar da 25.11 para a 26.05).*

Mude o "trilho" (canal): 
```bash
sudo nix-channel --add https://nixos.org/channels/nixos-26.05 nixos
```

Atualize a lista: 
```bash
sudo nix-channel --update
```

Migre o sistema: 
```bash
sudo nixos-rebuild switch --upgrade
```

**Passo 7:** O Botão de Pânico (Segurança):
*Se você fizer algo errado e o sistema não ligar ou o ambiente gráfico sumir:*

Reinicie o computador.

No menu inicial (Boot), escolha "NixOS - All configurations".

Selecione a versão de ontem ou a última que funcionava.

O sistema iniciará normalmente. 

*Para tornar essa versão a "oficial" de novo, basta rodar sudo nixos-rebuild switch enquanto estiver nela.*

| Ação | Comando |
| :--- | :--- |
| **`Aplicar mudança na config`** | sudo nixos-rebuild switch |
| **`Atualização semanal`** | sudo nixos-rebuild switch --upgrade |
| **`Testar pacote rápido`** | nix-shell -p pacote |
| **`git commit -m "..."`** | Cria uma nova versão oficial do código com uma etiqueta descritiva. |
| **`Limpar versões antigas`** | sudo nix-collect-garbage -d |
| **`Desfazer última mudança`** | sudo nixos-rebuild switch --rollback |

*Parabéns pela instalação! O NixOS recém-instalado é como uma tela em branco. Aqui está o roteiro do que você deve fazer para deixar o sistema pronto para o uso diário:*

---




*Drivers de Vídeo (Se tiver placa dedicada):*

Nvidia: Adicione services.xserver.videoDrivers = [ "nvidia" ]; e hardware.opengl.enable = true;.

4. Habilite o "Não Livre" (Unfree)
*Muitos programas (como Google Chrome, Discord, Drivers Nvidia) são proprietários. Para permitir que o NixOS os instale, adicione esta linha fora de qualquer bloco (geralmente no topo ou final do arquivo):*
nixpkgs.config.allowUnfree = true;

*Obs.: Ele já vai estar habilitado, na instalação normal você vai marcar.*

**6. Configure a Limpeza Automática**
*Para não ter que se preocupar com o disco enchendo com versões antigas, adicione isso ao seu configuration.nix:*

nix.settings.auto-optimise-store = true;
nix.gc = {
  automatic = true;
  dates = "weekly";
  options = "--delete-older-than 30d";
};

*Dica de Ouro: Guarde uma cópia do seu arquivo configuration.nix no seu Google Drive ou GitHub. Se você precisar formatar o PC um dia, basta colar esse arquivo em um NixOS novo e, em 5 minutos, o seu PC estará exatamente como era antes.*


3. Rotina do Usuário (Cronograma)
Toda Semana: Rode sudo nixos-rebuild switch --upgrade para manter segurança e apps em dia.

Uma vez por mês: Rode sudo nix-collect-garbage -d para não lotar o HD com versões antigas.






5. Regras de Ouro
Não reinicie à toa: Só é necessário para Kernel, Drivers de vídeo ou Bootloader. O switch cuida do resto.

Use o Rollback: Se algo quebrar, escolha a versão anterior no menu de boot. É o seu "ponto de restauração" garantido.

Use a Busca: Pesquise nomes de pacotes em search.nixos.org.

