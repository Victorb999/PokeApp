<template>
    <nb-container :style="{flex:1, backgroundColor: '#fff'}">
        <nb-header >
          <nb-left>
            <nb-button
              transparent
              :on-press="() => navigation.navigate('DrawerOpen')"
            >
                <nb-icon name="menu"/>
                </nb-button>
            </nb-left>
            <nb-body> 
                <nb-title>PokeApp</nb-title>
            </nb-body>
            <nb-right />
        </nb-header>
        <nb-content>
            
                <Pokeagenda />
               
            
        </nb-content>
    </nb-container>
</template>

<script>
import React from 'react';
import Item from '../components/item';
import Pokeagenda from '../screen/Pokeagenda';
import { Dimensions } from 'react-native';

const SCREEN_WIDTH = Dimensions.get('window').width;
export default {
  props: {
      navigation: Object
  },  
  components: {
      Item,
      Pokeagenda
  }
};
</script>