<template>
    <div class="location">
        <section>
            <p>自动定位</p>
            <p  @click="clickCity(current)">{{current.CityName}}</p>
        </section>
        <section>
            <p>省份</p>
            <ul>
                <li v-for="(item) in provinceList" :key="item.CityID" @click="clickProvince(item.CityID)">{{item.CityName}}</li>
            </ul>
        </section>

        <transition name="slideleft">
            <section class="city" v-if="showCityList">
                <div class="mask" @click.self="showCityList=false"></div>
                <ul class="list">
                    <li v-for="(item) in cityList" :key="item.CityID" @click="clickCity(item)">{{item.CityName}}</li>
                </ul>
            </section>
        </transition>
    </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from '@vue/composition-api';
import useLocation from '@/hooks/useLocation'

export default defineComponent({
  setup(props, {emit}) {
    const {provinceList, cityList, getCityListAction, current} = useLocation();
    const showCityList = ref(false)

    onMounted(() => {
        getCityListAction();
    })

    async function clickProvince(provinceid: number) {
        await getCityListAction(provinceid);
        showCityList.value = true;

    }

    function clickCity(city: {}) {
        emit('select', city);
    }

    return {
        provinceList,
        cityList,
        showCityList,
        current,
        clickProvince,
        clickCity
    }
  }
})
</script>

<style lang="scss" scoped>
@import "../scss/_mixin.scss";

.location,.city {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow-y: scroll;
  background: #fff;
}
section p:first-child{
    height: .4rem;
    line-height: .4rem;
    font-size: .24rem;
    padding-left: .2rem;
    background: #f4f4f4;
}
section p:last-child, section li{
    margin-left: .1rem;
    padding-left: .4rem;
    font-size: .28rem;
    height: .8rem;
    line-height: .8rem;
    background: #fff;
    position: relative;
}
section li{
    @include border1px();
    @include arrow(right, .2rem, #ccc);
}
.city{
    background: transparent;
}
.city .mask{
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.4);
}
.city .list{
    width: 70%;
    margin-left: 30%;
    height: 100%;
    overflow-y: scroll;
    position: absolute;
    top: 0;
    background: #fff;
}
</style>