# react-basket

[![Build Status](https://travis-ci.org/mbrn/react-basket.svg?branch=master)](https://travis-ci.org/mbrn/react-basket)
[![npm package](https://img.shields.io/npm/v/react-basket/latest.svg)](https://www.npmjs.com/package/react-basket)
[![NPM Downloads](https://img.shields.io/npm/dt/react-basket.svg?style=flat)](https://npmcharts.com/compare/react-basket?minimal=true)
[![Install Size](https://packagephobia.now.sh/badge?p=react-basket)](https://packagephobia.now.sh/result?p=react-basket)
[![Follow on Twitter](https://img.shields.io/twitter/follow/baranmehmet.svg?label=follow+baranmehmet)](https://twitter.com/baranmehmet)

A shopping basket components library for React based on material-ui components. 

## Installation
    npm install react-basket

## Usage

### 1. Implement a DataProvider

```javascript
import { DataProvider, BasketItem } from "react-basket";

export class MyBasketDataProvider implements DataProvider {
 
  registerToChanges(callback: (items: BasketItem[]) => void) {
      // You can call callback functions if you socket.io/pusher or something like it.  
  }

  products = [
    { id: "1", name: 'Computer', price: 1722.44, quantity: 1 },
    { id: "2", name: 'Phone', price: 522.14, quantity: 1 }
  ];

  items = [
    { id: "1", name: 'Computer', price: 1722.44, quantity: 1 },
    { id: "2", name: 'Phone', price: 522.14, quantity: 2 }
  ];

  getInitialData = (): Promise<BasketItem[]> => {
    return new Promise<BasketItem[]>((resolve, reject) => {
      setTimeout(() => {
        resolve(this.items)
      }, 1000)
    });
  }

  onAllItemsDeleted(): Promise<BasketItem[]> {
    return new Promise<BasketItem[]>((resolve, reject) => {
      setTimeout(() => {
        this.items = [];

        resolve(this.items)
      }, 1000)
    });
  }

  onItemAdded(id: string): Promise<BasketItem[]> {
    return new Promise<BasketItem[]>((resolve, reject) => {
      setTimeout(() => {
        let index = this.items.findIndex(item => item.id === id);
        if (index > -1) {
          this.items[index].quantity++;
        }
        else {
          const index = this.products.findIndex(item => item.id === id);
          if (index > -1) {
            const item = {...this.products[index]};
            this.items.push(item);
          }
        }

        resolve(this.items)
      }, 1000)
    });
  }

  onItemDeleted = (id: string): Promise<BasketItem[]> => {
    return new Promise<BasketItem[]>((resolve, reject) => {
      setTimeout(() => {

        const index = this.items.findIndex(item => item.id === id);
        if (index > -1) {
          this.items.splice(index, 1);
        }

        resolve(this.items)
      }, 1000)
    });
  }
}
```

### 2. Adding Basket Provider to app
    You should add BasketProvider component in root of your application with a data provider implementation. 

```javascript
class App extends React.Component {
  render() {
    const { classes } = this.props;

    return (
      <BasketProvider dataProvider={new MyBasketDataProvider()}>
        <div> your all components will be here </div>
      </>BasketProvider>
    );
  }
}
```

