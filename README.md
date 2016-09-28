## Как я ставлю свою систему

### Образ Федоры

Скачиваем образ:

```sh
wget https://download.fedoraproject.org/pub/fedora/linux/releases/24/Workstation/x86_64/iso/Fedora-Workstation-Live-x86_64-24-1.2.iso
```

Записываем его на USB-флешку:

```sh
sudo dnf install liveusb-creator
sudo liveusb-creator
```

Если мы на винде то используем Rawrite32



### Установка

1. Английскую раскладку на первое место.
2. Сеть и имя узла: «flatline».

3. Пользовать:
   - Полное имя: `Pasha Grekovich`
   - Имя пользователя: `pgrekovich`
   - Сделать администратором

### Обновление системы


Удаляем ненужные пакеты:

```sh
sudo dnf remove gedit cheese devassistant evolution evolution-ews evolution-help bijiben rhythmbox shotwell gnome-boxes gnome-documents gnome-weather empathy vinagre orca gnome-contacts yelp samba-client gnome-getting-started-docs nautilus-sendto gnome-shell-extension-* libreoffice-* setroubleshoot* gnome-characters
```

Подключаем RPM Fusion:

```sh
su -c 'dnf install --nogpgcheck http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm'
```

Подключаем Russian Fedora:

```sh
su -c 'dnf install --nogpgcheck http://mirror.yandex.ru/fedora/russianfedora/russianfedora/free/fedora/russianfedora-free-release-stable.noarch.rpm http://mirror.yandex.ru/fedora/russianfedora/russianfedora/nonfree/fedora/russianfedora-nonfree-release-stable.noarch.rpm http://mirror.yandex.ru/fedora/russianfedora/russianfedora/fixes/fedora/russianfedora-fixes-release-stable.noarch.rpm'
```

Подключаем репозиторий со свежим ядром:

```sh
curl -s https://repos.fedorapeople.org/repos/thl/kernel-vanilla.repo | sudo tee /etc/yum.repos.d/kernel-vanilla.repo
sudo dnf --enablerepo=kernel-vanilla-stable update
```

Обновляем систему:

```sh
sudo dnf update --refresh
```

Перезагружаемся.

### Настройка GNOME

Открываем Настройки:

- **Поиск:** выключаем «Nautilus», «Терминал» и «Центр приложений».
- **Мышь и сенсорная панель:** чувствительность на максимум,
  включаем «Естественная прокрутка» для тачпада.
- **Энегропитание:** выключаем «Уменьшать яркость при простое»,
  ставим «Выключение экрана» в «Никогда».
- **Пользователи:** ставим аватарку из этой папки.
* **Клавиатура:** заходим в «Комбинации клавиш» и очищаем «Сохранять снимок …».

Выставляем настройки клавиатуры, переключение на win+space, отключаем «caps lock»:

```sh
dconf write /org/gnome/desktop/input-sources/xkb-options "['grp:win_space_toggle', 'grp_led:caps', 'lv3:ralt_switch', 'misc:typo', 'nbsp:level3', 'caps:none']"
```

В Терминале:

- Параметры → Общие: выключить «Показывать панель меню …».
- Параметры профиля → Общие: выключить «Продавать гудок».

В Nautilus:

- Параметры → Вид: включить «Помещать папки перед файлами».
- Параметры → Поведение: включить «Открыть объекты одним щелчком».

Выключаем сканирование ФС:

```sh
dconf write /org/freedesktop/tracker/miner/files/crawling-interval -2
```

### VPN

Рабочий VPN:

Создаём VPN-соединие:

1. Settings → Network → + → VPN → OpenVPN.
2. Название: «Work VPN».
3. Указываем шлюз
4. Тип: «Password with Certificates (TLS)».
5. Указываем сертификаты и приватный ключ из скаченной папки.
6. Advanced → General:
  - Use LZO data compression
  - Use a TCP connection
  - Set virtual device type: `TUN` and name: `tun`
  - Use custom tunnel Maximum Transmission Unit `1500`
  - Restrict tunnel TCP Maximum Segment Size
7. Advanced → Security
  - Cipher: `AES-128-CBC`
  - HMAC Authentication: `SHA-1`



### Браузеры

Ставим для Фаерфокса расширения:

- [HTitle](https://addons.mozilla.org/RU/firefox/addon/htitle/)
- [GNOME 3](https://addons.mozilla.org/ru/firefox/addon/adwaita/)

Ставим Хром:

```sh
wget https://dl.google.com/linux/linux_signing_key.pub
sudo rpm --import linux_signing_key.pub
rm linux_signing_key.pub
sudo dnf install https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
```

Настройки: «Раннее открытые вкладки».
Показать дополнительные настройки: выставить «Рабочий стол» в Скаченные файлы.

Авторизоваться в Хроме. Авторизоваться в Твиттере, ГитХабе, Гиттере, Слаках,
ВКонтакте, Фидли, Фейсбуке.

### Внешний вид


Ставим расширения из `GNOME.md`. Добавляем Торонто в часы.

Установить шрифт Fira Mono и Fire Code:

```sh
sudo dnf install mozilla-fira-mono-fonts
mkdir -p ~/.fonts/FiraCode/
```

Скачиваем файлы FiraCode с репозитория в новую папку.

```
git clone git@github.com:tonsky/FiraCode.git
fc-cache
```

Установить иконки и тему:

```sh
sudo dnf config-manager --add-repo http://download.opensuse.org/repositories/home:snwh:moka/Fedora_24/home:snwh:moka.repo
sudo dnf install moka-icon-theme

dnf config-manager --add-repo http://download.opensuse.org/repositories/home:Horst3180/Fedora_24/home:Horst3180.repo
dnf install arc-theme
```

Улучшаем рендер шрифтов:

```sh
sudo dnf install freetype-freeworld
```

Установить GNOME Tweek Tool:

```sh
sudo dnf install gnome-tweak-tool
```

И выставить в нём настроки:

- **Верхняя панель:** включить «Показывать дату» и «Показывать секунды».
- **Внешний вид:** иконки выставить в «Moka» тему в «Arc».
- **Рабочий стол:** включить «Показывать значки на рабочем столе» и выключить
  все стандартные иконки.
- **Шрифты:** заголовок окон в «PT Sans Bold», интерфейс в «PT Sans Regular»,
  моноширный в «Fira Code», хиттинг в Slight
- **Электропитание:** выставить «При нажатии кнопки выключения» в «Blank».

### Кодеки и шрифты

Устанавливаем кодеки:

```sh
sudo dnf install amrnb amrwb faac faad2 flac gstreamer1-libav gstreamer1-plugins-bad-freeworld gstreamer1-plugins-ugly gstreamer-ffmpeg gstreamer-plugins-bad-nonfree gstreamer-plugins-espeak gstreamer-plugins-fc gstreamer-plugins-ugly gstreamer-rtsp lame libdca libmad libmatroska x264 xvidcore gstreamer1-plugins-bad-free gstreamer1-plugins-base gstreamer1-plugins-good gstreamer-plugins-bad gstreamer-plugins-bad-free gstreamer-plugins-base gstreamer-plugins-good
```

Устанавливаем программы:

```sh
sudo dnf install man-pages-ru mpv gimp unrar p7zip p7zip-plugins inkscape transmission-gtk
```


Spotify

```sh
dnf config-manager --add-repo=http://negativo17.org/repos/fedora-spotify.repo
dnf install spotify-client
```


Устаналивливаем шрифты от Microsoft:

```sh
sudo dnf install https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
```

### Папки


Чистим закладки:

```sh
echo "" > ~/.config/gtk-3.0/bookmarks
```

Удаляем лишние папки:

```sh
rm -R Видео Документы Изображения Музыка Общедоступные
```

### Разработка


Устанавливаем пакеты:

```sh
sudo dnf install git gitg ack redis
```


Устаналиваем `node` и `npm`:

```sh
wget https://nodejs.org/dist/latest/node-v6.6.0-linux-x64.tar.gz
sudo tar --strip-components 1 -xzvf node-v* -C /usr/local
rm node-*.xz
```


Устанавливаем Go:

```sh
sudo dnf install golang
mkdir -p ~/.go
go get -u -f github.com/DarthSim/hivemind
```

Устанавливаем VirtualBox:

Пишем в файл `/etc/yum.repos.d/virtualbox.repo`:

```sh
[virtualbox]
name=Fedora $releasever - $basearch - VirtualBox
baseurl=http://download.virtualbox.org/virtualbox/rpm/fedora/$releasever/$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://www.virtualbox.org/download/oracle_vbox.asc
```
Устанавливаем:

```sh
dnf install VirtualBox
```

Устанавливаем Vagrant:

```sh
## Ставим зависимости ##
sudo dnf install ruby-devel redhat-rpm-config zlib-devel
## Ставим Vagrant ##
sudo dnf install vagrant
## Ставим плагины ##
vagrant plugin install vagrant-cachier
```

### Текстовые редакторы

Устанавливаем nano:

```sh
sudo dnf install nano
su -c 'echo "export EDITOR=nano" >> /etc/profile'
su -c "echo '
set autoindent
include \"/usr/share/nano/*.nanorc\"' >> /etc/nanorc"
```

Устанавливаем Vim:
```sh
sudo dnf install vim
```

Установить Атом:

```sh
wget https://atom.io/download/rpm -O atom.rpm
sudo dnf install atom.rpm
rm atom.rpm
```

Устанавливаем темы и плагины из `Atom.md`.

Устанавливаем ngrok. Скачиваем [архив](https://ngrok.com/download).

```
unzip ngrok.zip
sudo cp ./ngrok /usr/local/bin/
```

### zsh

Устанавливаем zsh:

```sh
sudo dnf install zsh
chsh -s /usr/bin/zsh
rm ~/.bash_history ~/.bash_logout
```


Устанавливаем oh-my-zsh:

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Устанавливаем тему для zsh:

```sh
  curl -o - https://raw.githubusercontent.com/denysdovhan/spaceship-zsh-theme/master/install.sh | zsh
```
### Чаты
##### Skype

```sh
## Install Needed Dependencies ##
dnf install alsa-lib.i686 fontconfig.i686 freetype.i686 \
glib2.i686 libSM.i686 libXScrnSaver.i686 libXi.i686 \
libXrandr.i686 libXrender.i686 libXv.i686 libstdc++.i686 \
pulseaudio-libs.i686 qt.i686 qt-x11.i686 zlib.i686 qtwebkit.i686


cd /tmp

wget --trust-server-names http://www.skype.com/go/getskype-linux-dynamic

mkdir /opt/skype

tar xvf skype-4.3* -C /opt/skype --strip-components=1

## Link skype.desktop ##
ln -s /opt/skype/skype.desktop /usr/share/applications/skype.desktop

## Link icons (copy and paste all rows at once) ##
for icon in /opt/skype/icons/*; do
ressuffix="${icon##*_}"
res="${ressuffix%%.*}"
ln -s "$icon" /usr/share/icons/hicolor/"$res"/apps/skype.png
done

## Update gtk icon cache (needed at least Gnome/GTK envorinments) ##
gtk-update-icon-cache /usr/share/icons/hicolor

## Update gtk icon cache (needed at least Gnome/GTK envorinments) ##
touch /usr/bin/skype
chmod 755 /usr/bin/skype
```

Открываем в vim файл `/usr/bin/skype` и добавляем:

```sh
  #!/bin/sh
  export SKYPE_HOME="/opt/skype"

  $SKYPE_HOME/skype --resources=$SKYPE_HOME $*
```

В настройках скайпа выключаем звук на отправку сообщения, выключаем все уведомления кроме входящего звонка, файла, сообщения.
Включаем `Display built-in system pop-up notification`

#### Telegram
https://slack.com/downloads/instructions/fedora
```sh
sudo dnf copr enable rommon/telegram
sudo dnf install telegram-desktop
```

#### Slack

```
Качаем и ставим с сайта:
https://slack.com/downloads/linux
```
