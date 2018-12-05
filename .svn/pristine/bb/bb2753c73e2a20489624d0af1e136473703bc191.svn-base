import Login from './views/Login.vue'
import NotFound from './views/404.vue'
import Home from './views/Home.vue'
import Main from './views/Main.vue'
import Room from './views/nav1/room.vue'
import Spring from './views/nav1/spring.vue'
import Res from './views/nav1/res.vue'
import Meeting from './views/nav1/meeting.vue'
import Search from './views/nav1/search.vue'
import Rate from './views/charts/rate.vue'
import Origin from './views/charts/origin.vue'

let routes = [
    {
        path: '/login',
        component: Login,
        name: '',
        hidden: true
    },
    {
        path: '/404',
        component: NotFound,
        name: '',
        hidden: true
    },
    //{ path: '/main', component: Main },
    {
        path: '/',
        component: Home,
        name: '列表',
        iconCls: 'el-icon-message',//图标样式class
        children: [
            { path: '/main', component: Main, name: '主页', hidden: true },
            { path: '/room', component: Room, name: '客房' },
            { path: '/spring', component: Spring, name: '温泉' },
            { path: '/res', component: Res, name: '餐饮' },
            { path: '/meeting', component: Meeting, name: '会议室' },
            { path: '/search', component: Search, name: '算法' },
        ]
    },
    {
        path: '/',
        component: Home,
        name: '分析',
        iconCls: 'fa fa-bar-chart',
        children: [
            { path: '/rate', component: Rate, name: '入住率' },
            { path: '/origin', component: Origin, name: '客源' }
        ]
    },
    {
        path: '*',
        hidden: true,
        redirect: { path: '/404' }
    }
];

export default routes;