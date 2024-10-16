# Magento Docker Compose
## Minimal docker setup
Only comes with essential components to get magento running, perfect for new developer with a linux machine

## Requirement
* docker engine https://docs.docker.com/engine/install/ (not docker desktop)
* docker compose https://docs.docker.com/compose/install/linux/

## Installtion
1. clone or download this repo to a project folder
`
cd ~
git clone https://github.com/SunMerce/magento-docker myapp
`
2. in the project folder, clone or download magento into a web folder
`
cd myapp
git clone https://github.com/mage-os/mageos-magento2
`
3. rename nginx config file
`
cp -n web/nginx.cong.sample web/nginx.cong
`
4. build docker containers
`
docker compose build
`
5. run docker containers
`
docker compose run
`

now you can access your magento in your browser, by default the url will be [http://http://127.0.0.1:81/](http://127.0.0.1:81/)

## FAQ
### Is it Linux only?
It technically would run in other operating system as well, but this project does not contain the optimizing options tailered for other system. You may experience suboptimal performance. I strongly recommend developers migrate to a linux based system for magento development.
### How do I run a local domain and/or HTTPS
If you prefer having a domain or SSL for local developemtn, use a reverse proxy. I find caddy works great for me.
### How do I setup multiple magento instances for my different clients/projects
In each of your project, modify the .env file to give the project a unique prefix and a set of unique port numbers
### How to send email / configure SMTP
I recommend using a free developer tool like https://mailtrap.io, get SMTP credentails from your account, and configure in magento admin https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications
