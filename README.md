# Chasm-Network

> Yarın uçuyorum haliyle repo paylaşamazdım, puandan geri kalmamanız için gece atıyorum üzgünüm.

#

> Chasm Network Scout Node'umu kurup puan toplamaya başladım, detaylar:

> Donanım minimal herhangi bir sunucu yeterli.

> Tek bir node run ediyoruz.

#

### Kurulum

```console
sudo apt update && sudo apt upgrade -y

for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update


sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo docker run hello-world
```

#

> Testnet cüzdanımızda fee'lik Mantle token olmalı 0.10$'lık yetiyor sanırım.

> Sonra [buradan](https://scout.chasm.net/private-mint) bir Chasm Season hesap oluşturup mintleyelim ve setup yapalım.

> Bazı hesaplar açmalıyız, sırasıyla:

> [Groq](https://console.groq.com/keys) - [OpenRouter](https://openrouter.ai/settings/keys) - [OpenAI](https://platform.openai.com/api-keys).

#

```console
nano .env
# nano ile .env içine girdik.
# alttaki code bloğunu içine yapıştırdık.
```

> Düzenlemeniz gereken yerleri kod bloğunun altına yazıyorum.

```console
PORT=3001
LOGGER_LEVEL=debug

# Chasm
ORCHESTRATOR_URL=https://orchestrator.chasm.net
SCOUT_NAME=
SCOUT_UID=
WEBHOOK_API_KEY=
# Scout Webhook Url, update based on your server's IP and Port
# e.g. http://123.123.123.123:3001/
WEBHOOK_URL=

# Chosen Provider (groq, openai)
PROVIDERS=groq
MODEL=gemma2-9b-it
GROQ_API_KEY=

# Optional
OPENROUTER_API_KEY=
OPENAI_API_KEY=
```

> Scout Name belirleyin.

> Scout ID'iniz [Chasm Season](https://scout.chasm.net/new-scout) hesabınızda blurlu olan yerleri mausunuzu getirin gözükecek.

> Webhook API'da aynı şekilde.

> Webhook URL: http://IP_ADRESİNİZ:3001/ - IP adresiniiz düzenleyip yapın.

> Groq API Groq hesabınızda.

> Openrouter ve OpenAI keyleride yukarıda oluşturdunuz oradan alınız.

#

> Akabinde CTRL + X + Y ENTER ile kaydedip çıkınız.

#

```console
# nodeumuzu başlatalım.
docker pull johnsonchasm/chasm-scout
docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout johnsonchasm/chasm-scout

source ./.env
curl -X POST \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer $WEBHOOK_API_KEY" \
     -d '{"body":"{\"model\":\"gemma-7b-it\",\"messages\":[{\"role\":\"system\",\"content\":\"You are a helpful assistant.\"}]}"}' \
     $WEBHOOK_URL

# bu komutla OK çıktısı aldıysak tamamdır.
curl localhost:3001

# komut çalışmazsa bu komutun çıktısında ki linke tıklayıp sol üste bakın.
docker logs scout
```

> Kurulum başarılıysa listenin sonunda adınızı göreceksiniz [burada](https://scout.chasm.net/leaderboard).

<img width="1318" alt="Ekran Resmi 2024-07-20 00 44 02" src="https://github.com/user-attachments/assets/45a468af-d186-4e2c-9f9b-614d5de92091">



