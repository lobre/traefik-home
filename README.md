# Traefik Home

This tool acts as a home page for referencing containers exposed through `traefik`. It uses `docker-gen` to watch containers events and to generate a frontend page published with `nginx`. The final HTML page is stylised using `bootstrap`.

> Træfik (pronounced like traffic) is a modern HTTP reverse proxy and load balancer made to deploy microservices with ease.

See.
 - https://traefik.io/
 - https://github.com/jwilder/docker-gen
 - http://getbootstrap.com/

# Quick start

    docker run --name traefik-home -v /var/run/docker.sock:/tmp/docker.sock:ro \
        --label traefik.enable=true \
        --label traefik.port=80 \
        --label traefik.frontend.rule=Host:traefik-home.example.com \
        --label traefik.home=false \
        lobre/traefik-home

# Docker compose

    version: '3'

    services:
      nginx:
        image: lobre/traefik-home
        container_name: traefik-home
        volumes:
          - /var/run/docker.sock:/tmp/docker.sock:ro
        labels:
          - traefik.enable=true
          - traefik.port=80
          - traefik.frontend.rule=Host:traefik-home.example.com
          - traefik.home=false

# Labels to configure Home

The `traefik_home` container can be configured using the following labels.

| Label  | Description |
| ------------- | ------------- |
| traefik.home.navbgcolor=#89a3ff | Background color of the nav bar |
| traefik.home.navtextcolor=#000 | Text color of the nav bar title |
| traefik.home.title=My Title | Nav bar title |

# Volumes to configure Home

You can mount a few volumes to replace the CSS, main icon or favicon

    volumes
      - ./mylogo.png:/usr/share/nginx/html/logo.png
      - ./mystyle.css:/usr/share/nginx/html/style.css
      - ./favicon.ico:/usr/share/nginx/html/favicon.ico

# Labels to configure containers

Home will use the following `traefik` labels to generate the HTML page.

| Label  | Description |
| ------------- | ------------- |
| traefik.frontend.rule=my-container.example.com  | URL used by Home to generate the link |
| traefik.frontend.entryPoints=https,http | Protocol determined through this label (by default HTTP) |
| traefik.frontend.auth.basic=user:md5password | A lock icon is display if this label is filled |

When exposing containers through `traefik`, the following labels can be added to add information on the Home page.

| Label  | Description |
| ------------- | ------------- |
| traefik.home=true  | Container shown/hidden on HTML page if set to true/false |
| traefik.home.category=My Category  | Containers grouped by categories if defined |
| traefik.home.image=http://urlofimage.png  | URL of image for icon. If no image precised, a icon with the container's initials is generated |
| traefik.home.alias=My Alias  | If filled, the alias is taken instead of the container name for the icon label |
| traefik.home.description=My Description  | If filled, a description is displayed below the icon |