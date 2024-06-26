# GIN

## 拦截器

Gin的中间件如果使用拦截器，可以自定义修改前端传入后端的字段，以让后端方便匹配更多的前端，增强兼容性

ModifyApiFieldName

目前支持Query和Params的值

举个例子，比如你想让devtool开发的后端匹配dev-admin-web，你需要设置以下配置

```yaml
api:
    field:
        active_path: activePath
        enter_transition: enterTransition
        extra_icon: extraIcon
        fixed_tag: fixedTag
        frame_loading: frameLoading
        frame_src: frameSrc
        hidden_tag: hiddenTag
        keep_alive: keepAlive
        leave_transition: leaveTransition
        menu_type: menuType
        page: currentPage
        page_size: pageSize
        parent_id: parentId
        show_link: showLink
        show_parent: showParent
        token: accessToken
```
