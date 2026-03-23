# Comandos Úteis para Servidor Linux e Docker

Este documento reúne comandos essenciais para administração de servidores Linux e para trabalhar com Docker. Use como referência rápida.

---

## 1. Comandos Básicos do Linux

| Comando | Descrição |
|---------|-----------|
| `ls -la` | Lista arquivos com detalhes (incluindo ocultos) |
| `cd /caminho` | Muda para o diretório especificado |
| `pwd` | Mostra o diretório atual |
| `cp -r origem destino` | Copia arquivos ou diretórios recursivamente |
| `mv origem destino` | Move ou renomeia arquivos/diretórios |
| `rm -rf arquivo` | Remove arquivos/diretórios forçadamente (cuidado!) |
| `mkdir nome` | Cria um diretório |
| `chmod 755 arquivo` | Altera permissões (exemplo: rwxr-xr-x) |
| `chown usuario:grupo arquivo` | Altera proprietário e grupo |
| `touch arquivo` | Cria um arquivo vazio ou atualiza timestamp |

---

## 2. Gerenciamento de Processos

| Comando | Descrição |
|---------|-----------|
| `ps aux` | Lista todos os processos em execução |
| `top` ou `htop` | Monitor de processos interativo (htop requer instalação) |
| `kill -9 PID` | Finaliza um processo pelo PID |
| `systemctl status serviço` | Verifica status de um serviço systemd |
| `systemctl start/stop/restart serviço` | Controla serviços systemd |
| `journalctl -u serviço -f` | Acompanha logs de um serviço em tempo real |

---

## 3. Rede

| Comando | Descrição |
|---------|-----------|
| `ip a` | Exibe configurações de rede (substituto do ifconfig) |
| `ss -tulpn` | Mostra portas abertas e processos associados |
| `ping host` | Testa conectividade |
| `curl -I http://site` | Exibe cabeçalhos HTTP |
| `wget arquivo` | Baixa arquivos via HTTP/HTTPS |
| `nc -zv host porta` | Verifica se uma porta está acessível (netcat) |

---

## 4. Usuários e Permissões

| Comando | Descrição |
|---------|-----------|
| `useradd -m usuario` | Cria um usuário com diretório home |
| `passwd usuario` | Altera a senha do usuário |
| `usermod -aG grupo usuario` | Adiciona usuário a um grupo secundário |
| `groupadd grupo` | Cria um novo grupo |
| `sudo comando` | Executa comando como superusuário |
| `su - usuario` | Muda para outro usuário (login completo) |

---

## 5. Gerenciamento de Pacotes (Debian/Ubuntu)

| Comando | Descrição |
|---------|-----------|
| `apt update` | Atualiza lista de pacotes |
| `apt upgrade` | Atualiza todos os pacotes instalados |
| `apt install pacote` | Instala um pacote |
| `apt remove pacote` | Remove um pacote (mantém configurações) |
| `apt purge pacote` | Remove pacote e configurações |
| `apt search termo` | Procura por pacotes |

**Para RHEL/CentOS (yum/dnf):**

| Comando | Descrição |
|---------|-----------|
| `yum install pacote` ou `dnf install pacote` | Instala um pacote |
| `yum update` | Atualiza todos os pacotes |

---

## 6. Monitoramento de Sistema

| Comando | Descrição |
|---------|-----------|
| `df -h` | Espaço em disco em formato legível |
| `du -sh *` | Tamanho de cada arquivo/pasta no diretório atual |
| `free -h` | Uso de memória RAM e swap |
| `uptime` | Tempo de atividade e carga média |
| `vmstat 1` | Estatísticas de sistema em tempo real |
| `iostat -x 1` | Monitor de I/O de disco |
| `dmesg \| tail` | Mensagens do kernel (últimas linhas) |

---

## 7. Docker – Comandos Essenciais

| Comando | Descrição |
|---------|-----------|
| `docker ps` | Lista containers em execução |
| `docker ps -a` | Lista todos os containers (incluindo parados) |
| `docker images` | Lista imagens locais |
| `docker pull imagem:tag` | Baixa uma imagem do registry |
| `docker run -d --name nome imagem` | Executa um container em background |
| `docker exec -it container bash` | Acessa o shell de um container em execução |
| `docker logs -f container` | Acompanha logs do container |
| `docker stop/start/restart container` | Controla o estado do container |
| `docker rm container` | Remove um container (parado) |
| `docker rmi imagem` | Remove uma imagem |
| `docker build -t nome .` | Constrói uma imagem a partir de um Dockerfile |
| `docker system prune -a` | Remove recursos não utilizados (containers, imagens, volumes) |

---

## 8. Docker – Volumes e Redes

| Comando | Descrição |
|---------|-----------|
| `docker volume create volume` | Cria um volume nomeado |
| `docker volume ls` | Lista volumes |
| `docker volume rm volume` | Remove um volume |
| `docker network create rede` | Cria uma rede personalizada |
| `docker network ls` | Lista redes |
| `docker network connect rede container` | Conecta um container a uma rede |
| `docker inspect container` | Exibe detalhes completos do container |

---

## 9. Docker Compose

| Comando | Descrição |
|---------|-----------|
| `docker-compose up -d` | Inicia os serviços em background |
| `docker-compose down` | Para e remove os containers, redes padrão |
| `docker-compose logs -f` | Acompanha logs de todos os serviços |
| `docker-compose exec serviço bash` | Executa comando em um serviço |
| `docker-compose ps` | Lista os containers do projeto |
| `docker-compose build` | Reconstrói as imagens sem iniciar |

---

## 10. Dicas e Soluções de Problemas

- **Liberar espaço em disco:**  
  `docker system prune -a` (remove dados não utilizados)  
  `sudo journalctl --vacuum-size=100M` (limpa logs antigos do systemd)

- **Verificar logs do Docker:**  
  `sudo journalctl -u docker -f`

- **Reiniciar serviço Docker:**  
  `sudo systemctl restart docker`

- **Encontrar processos que usam uma porta:**  
  `sudo lsof -i :80` ou `ss -tulpn | grep :80`

- **Verificar uso de memória por container:**  
  `docker stats --no-stream`

- **Acessar logs de um container específico:**  
  `docker logs --tail 100 -f container`

- **Executar um comando único em um container existente:**  
  `docker exec -it container comando`

- **Criar um alias para comandos frequentes (ex.: .bashrc):**  
  `alias dps='docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"'`

---

## Observações

- Sempre verifique permissões e evite usar `rm -rf` sem certeza.
- Em produção, nunca exponha portas desnecessárias e use usuários não-root nos containers.
- Utilize `--help` para obter detalhes de qualquer comando, ex.: `docker run --help`.
