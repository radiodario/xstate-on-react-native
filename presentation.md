---
marp: true
paginate: true
---

![bg left:30% 80%](https://raw.githubusercontent.com/statelyai/public-assets/main/logos/xstate-logo-black-nobg.svg)

# **[XState] in React Native**

## How I finaly managed to keep my views lean and my logic clean

[XState]: http://xstate.js.org/

---

1. The problem with apps
2. What are state machines
3. How to use them in your projects


---


![bg](ModularSynth.gif)

<!-- Who can tell me what this is -->
---

# WRONG!

This is a photo of a typical react native project


---

![bg](hoardersRoom.jpg)

<!-- Who can tell me what this is -->

---

# WRONG!

This is a photo of a typical redux store after a year


---

![bg](catsInBoxes.jpg)

<!-- Who can tell me what this is -->

---

# WRONG!

This just is a photo of a bunch of cats sitting in boxes

---


```!<--- pause for laughter --->```


---

# Face it, you make forms for a living.

---

# And making forms shouldn't be that hard
![bg left](formsBernard.gif)

---

# Enter State Machines

---

# What do these two have in common?
![bg right:70%](ParkingMeter.jpg)
![bg](fortnite-snake-eyes.gif)


---

<!-- _header: 'Modelling a cat' -->

![bg](CatStateMachine.png)

--- 

# Wrap your flow in a context
```tsx
const MyMachineContext = React.createContext<InterpreterFrom<typeof MyMachine>>();

function MyFlow = () => {

  const machine = useInterpreter(myMachineDefinition, {
    actions: {
      ...
    },
    services: {
      ...
    }
  })

  return (
    <MyMachineContext.Provider value={machine}>
      ...
    </MyMachineContext.Provider>
  )
}

```

---

# Use a hook to navigate

```ts
const useNavigateForMyViews = () => {
  const service = useContext(MyStateMachineContext)
  const navigation = useNavigation();

  useEffect(() => {    
    const subscription = service.subscribe((state) => {
      if (state.matches("authorizationExpired")) {
        navigation.navigate(
          "AuthorizationError", { 
            message: "Cannot authorise payments on this device.",
        });
      }
    }
    return subscription.unsubscribe;
  }, [service, navigation]);
}
```

--- 

# You get many things for free

```tsx

const service = useContext(MyStateMachineContext)
const service = useActor()


```

---

### Some things to keep in mind

* Autorefresh doesn't work
* Keep state / context lean

---

### References and further reading

* [State machines are wonderful tools - Chris Wellons](https://nullprogram.com/blog/2020/12/31/)
* [Rage Against the Finite-State Machines](https://learnyousomeerlang.com/finite-state-machines)
* [Integrating XState with React Native and React Navigation - Simone D'Avico](https://medium.com/welld-tech/integrate-xstate-with-react-native-and-react-navigation-21ead87391da)