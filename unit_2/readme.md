Ссылка на страницу с информацией о системе - http://your_IP_adress:8123/hassio/system   
Чтобы каждый раз не переходить по этой ссылке, на боковую панель можно вывести кнопку, для этого в configuration.yaml добавим:
```
panel_custom:
  - name: server_state
    sidebar_title: 'Система'
    sidebar_icon: mdi:server
    js_url: /api/hassio/app/entrypoint.js
    url_path: 'hassio/system'
    embed_iframe: true
    require_admin: true
    config:
      ingress: core_configurator 
```
