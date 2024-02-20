# react-render-builder

Simplify reusing Context-like components when testing Components and Hooks in React-Native

## Install

### yarn

```shell
yarn add react-render-builder
```

### npm

```shell
npm install react-render-builder
```

## Usage

```tsx
import React from 'react';
import { Text, View } from 'react-native';
import { describe, expect, it } from '@jest/globals';
import { CounterProvider, useCounter } from './CounterContext';
import { ReactRenderBuilder } from 'react-render-builder';


class RenderBuilder extends ReactRenderBuilder {
    counter(initialValue: number) {
        this.addElement(children => <CounterProvider initialValue={initialValue} children={children}/>);
        return this;
    }
}

function Hello() {
    const counterValue = useHelloHook();
    return (
        <View>
            <Text>{counterValue}</Text>
        </View>
    );
}

function useHelloHook() {
    const counterValue = useCounter();
    return `Hello ${counterValue.value}`;
}

describe('hello with counter 1', () => {
    const renderBuilder = new RenderBuilder().counter(1);

    it('render correct string in Hello Component', () => {
        const renderApi = renderBuilder.render(<Hello/>);
        renderApi.getByText('Hello 1');
    });

    it('returns correct string in useHelloHook', () => {
        const helloText = renderBuilder.renderHookResult(useHelloHook);
        expect(helloText).toEqual('Hello 1');
    });
});
```

## Documentation
