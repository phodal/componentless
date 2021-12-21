# Componentless: a architecture pattern for low-code age.

PS: This is a very fun front-end architecture model that can create unlimited fun. I wrote this article mainly because it is fun, and **there is nothing new**.

When I create [Quake](https://github.com/phodal/quake/) which is a meta-framework for knowledge management, with meta-data and quake component, you can combine any data to any component, like story in calendar with Transflow DSL `from('story').to(<quake-calendar>`, story is build from meta-data:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jciwm6pnxp0o1zc53k9u.png)


I found `componentless` is the pattern of Quake's low-code design principle, I decide to abstract the patterns of it. I call it Componentless:

> Componentless architecture is an architectural pattern, it refers to a large number of rely on third-party components (runtime dependent components rather than compile-time dependent components, that is compilation as a service) or temporary storage of custom code running in the container Frontend application. The three-party components of the application, like the three-party API service, can be independently released and deployed independently, and the application does not need to be recompiled, built, and deployed.

From the name Componentless, you can know that its target is the serverless-like back-end architecture pattern. Therefore, the definition is quite similar to that of the Serverless architecture. That's why, we define it as a componentless architecture. You don't need to write any components, you only need to write logic code or DSL to achieve their combination. Moreover, we can only provide a DSL + universal URL, and the browser will complete the automatic construction and operation of the application according to the DSL.

Quake online demo: [https://quake-demo.inherd.org/](https://quake-demo.inherd.org/), try typing `from('stroy').to(<quake-calendar>)`, the `<quake-calendar>` can be any `quake-component` (like `<quake-component>`, we only have two components in 2021.12.21) or any `ionic` component.

Quake source code: [https://github.com/phodal/quake/](https://github.com/phodal/quake/)

## Evolution of front-end and back-end architecture

Previously, it was quite interesting to often communicate with others about the application of domain-driven design (DDD) in the front-end. As a “nine and three-quarters”/10 DDD bricks, in the past, I always felt that domain-driven design is not suitable for the front-end. Clean front-end architecture is what people need, but design + getting started is slightly more difficult. 

In this year, I have used DDD design and planning for multiple back-end applications, and I have a new experience (although it still doesn't work). The front-end can have a DDD-like approach, but the approach is completely different from the back-end. The back-end uses model and function as the basis of two different programming styles, and the front-end uses components + events as the basis of the programming style. Components are destructible, and events are orchestrated by designing event streams.

![Clean Frontend](https://github.com/phodal/componentless/raw/master/images/clean-frontend.png)

Therefore, you don't directly apply the idea of back-end DDD to front-end applications, unless the logic of your application is focus on the front-end.

### Microservices and micro frontends

For most of today's systems, they still remain in a state of "back-end microservices, front-end "big mudball"." The back-end microservices have been disassembled into individual microservices according to "Conway's Law" (of course, unreasonably splitting the microservices is another problem), while the front-end is still in a state of a big mud ball. Therefore, the micro-frontend is used as one of the (not the only) technologies to solve the organizational structure alignment and implement the architectural model of rapid release and online. It can split a single large application into multiple smaller autonomous applications, but they are still aggregated into one. It can be used to solve the migration of legacy systems, unify user experience, help multi-team collaboration, etc.

When migrating back-end systems, we use DDD (Domain Driven Design) to find a reasonable basis for the design of microservice architecture. Microservices have become a way for us to transform the legacy system. We start with one module and one function, and gradually replace the old single application until the entire system is replaced. This replacement mode is quite similar for front-end applications.

Therefore, after the transformation of the micro front end, the structure is aligned, and the personnel is aligned. Everyone is happy.

![MS Align](https://github.com/phodal/componentless/raw/master/images/ms-align.png)

Going forward, how should we continue to evolve the system?

### Serverless and ComponentLess


In 2017, after learning about DDD and Serverless, and create the "Serverless Application Development Guide" (https://serverless.ink/), I have been thinking about how to apply Serverless-like ideas in the front-end? So, there was an idea about the cross-frame component library: "The second half of the front-end: building a cross-frame UI library", but these domestic companies that write component libraries do not have such a bold idea, it is a pity-only the version number + 1, what others do follow? There is also an interesting story line. After experiencing the enthusiasm of low-code programming, I rethinked the future of front-end and back-end: "[Front-end and back-end integration: Will the front-end and back-end separation die?](https://www.phodal.com/blog/kill-frontend-backend/)".

At first, I thought no-code programming was a ComponentLess direction, but a research found that it was not. Nocode programming tends to visualize programming, while Componentless tends to use DSL programming. At this point, I prefer to use Web Components + WASM technology to build a new front-end architecture.

Until I recently reapplied this idea in the open source knowledge management tool [Quake](https://github.com/phodal/quake), I found it particularly interesting, so I wanted to write an article to introduce the related idea-after all , The market has accepted the Serverless concept and the micro front end concept. Then, the remaining questions become very simple.

## Componentless architecture

Continue back to the definition at the beginning:

> Componentless architecture is an architectural pattern, it refers to a large number of rely on third-party components (runtime dependent components rather than compile-time dependent components, that is, compilation as a service) or temporary storage of custom code running in the container Front-end application. The three-party components of the application, like the three-party API service, can be independently released and deployed independently, and the application does not need to be recompiled, built, and deployed.

Simply, what a componentless thing needs to do is to turn the component into a **runtime service** instead of a compile-time dependency in the past. When all the components become a kind of infrastructure, we no longer need these components, and then let the components disappear from the application development side, and achieve a state where the application does not require components. In this way, it has also become a LowCode type system, with simple code generation, it can reach the state of NoCode.

From a formal point of view, the use of micro-front-end related technologies can provide a series of basic technologies required by a componentless architecture. Among them, the simplest way is to use: Web Components. So, let us first look at an example of a Componentless architecture based on Web Components.

### Example: How to move towards a Componentless architecture?

In terms of the process, it can be divided into three steps:

1. Decompose the application using Web Component
2. Split more components to eliminate components
3. Build a generative low-code model

The remaining part is fill-in-the-blank programming.

#### 1. Use Web Component to decompose the application

Let's look at an example first. For example, our front-end part has two micro-applications, A and B. The granularity is already very small, but it is still an application-level application. Application B is built using Web Components technology, and two tripartite Web Components components are introduced into micro-application B. In a conventional front-end application, if we update these two components, the corresponding application needs to be recompiled and released again.

![CLS Slot](https://github.com/phodal/componentless/raw/master/images/cls-slot.png)

For now, with the support of Custom Element + Shadow DOM, we only need to update the link to the script tag of the component library, or the cache.

#### 2. Split more components to eliminate components

Next, let us further optimize, remove all the internal components of application A and application B, externally build these components into a set of components according to functional modules. These component sets can be divided by functional teams.

![Step 2](https://github.com/phodal/componentless/raw/master/images/cls-step-2.png)

These are not important. Now there are very few "components" in our application-we still have some components for orchestrating these components + some additional business logic.

#### 3. Build a generative low-code model

Now, let’s review the "hello, world" function written in Serverless (AWS Lambda, they don’t pay for advertising):

```javascript
module.exports.hello = (event, context, callback) => {
   callback(null, "hello, world");
};
```

When using a framework like Serverless Framework, we only need to fill in our business logic on this function, that is, fill-in-the-blank programming. For the front end, the process is similar. We have data and our target components. Only a limited code generation function is needed. That is, we only need to generate an empty function to be improved, such as Transflow in Quake: `from('todo','blog').to(<quake-calendar>)`, the generated function and logic (part of the code example ):

```javascript
const tl_temp_1 = async (context, commands) => {
const el = document.createElement('quake-calendar');
    ...
return el;
}
```

At this time, you only need to ensure that the routing and functions are not modified, and the remaining part is to fill in the blanks for data processing.

### Migration to Componentless

In addition to the above-mentioned direct decomposition method, there are other gradual migration methods.

Migration method 2: new embedded in old

1. Use new technologies and frameworks to create application shelves.
2. Extract the Web Component and insert it into the old component, and then change the public capabilities.
3. Embed old wheels in new applications.

Migration method 3: old embedded in new

1. Build a new Web Component component. Cooperate with monorepo management
2. Embed components into existing applications.
3. Improve the componentless architecture mechanism.
4. Build a low-code orchestration model.

### Componentless architecture concept

From the current personal understanding, its core concept is: **Components are "services."** That is, components can be deployed and updated freely, just like services. After the component is updated, the application has also reached the update of the application in a sense.

In addition, there are such as:

1. Automated environment isolation. Christmas is coming soon
2. Generate low code. The real front end glue

More content remains to be explored.

### Componentless architecture issues

In addition to the many advantages mentioned above, it also has a series of shortcomings to be solved:

1. Browser compatibility. Web Component2 compatibility issues
2. Test difficulty. Free architecture often means the cost of testing, which is similar to microservices and serverless at this point. More end-to-end testing will be required to ensure the quality of the project.
3. The division basis of component modularization. When we build a set of components, we need to find a way to plan rationally.
4. Monorepo management. The more repo, the more complicated the management. Need to introduce tools such as nx and pnpm for management.
5. Upgrade strategy. That is, the upgrade strategy of the application and the component set should remain inconsistent.
    ...

### Advantage scenario: combined with lowcode

In a sense, componentless architecture is a generalized low-code implementation mode. Because of the more independent component model, the low-code system it builds is more interesting:

1. Configuration is runtime. It is similar to the process-oriented style of Oracle DB, and realizes new features quickly on the line.
2. Fill-in-the-blank programming for code generation. As mentioned in the above example, basic function codes can be generated, and then developers can add code logic.
3. Low code based on stream orchestration. The same applies to the traditional low-code architecture model.
4. DSL style low code. Such as Quake based on DSL to build.

It's just that, in terms of mode, there is not much difference.

## Componentless patterns

None of the above is interesting. After we adopt Web Component as the implementation technology of componentless architecture, there will be more room for architectural display. Web Components is already a very good container similar to Docker, which can play various fancy containerization modes. We tried some patterns at Quake, which brought a series of challenges, but it was also very interesting.

### Adapter: Compatible with existing components.

Based on the features of WC, encapsulating the components of the existing mainstream frameworks such as Angular, React, Vue, etc., can quickly provide such capabilities. For example, the QuakeTimeline and QuakeCalendar we provide in Quake are all in this way. React components are packaged as Web Components:

```javascript
class ReactElement extends HTMLElement {
...
}
customElements.define('quake-calendar', ReactElement);
```

Since the WC components are exposed to the outside, it doesn't matter what front-end framework is used.

### Ambassador pattern

In the cloud-native model, the Ambassador model can create services or applications on behalf of consumers and send help services for network requests. The same event can also be encapsulated by components,

```javascript
const fetchEl = document.createElement('fetch-api');
fetchEl.setAttribute("url", "/action/suggest);
fetchEl.addEventListener("fetchSuccess", (res: any) => {
let response = res.detail;
loading.onDidDismiss().then(() => {});
callback(response);
})
```

However, I wrote this just for fun. I created a Loading component and inserted the `<fetch-api>` component in Loading to initiate an HTTP request. After the request was successful, the DOM was destroyed.

In this way, I only need to replace this request component to replace all request APIs.

### Infinite nesting "Dolls" patten

In the normal pattenrn, we call the B component in the A component, then in theory, we don't need to call the A component in the B component, it will form a circular reference, but it becomes a function in Web Components.

For example, in Quake's markdown rendering engine `<quake-render>`, the `<embed-link>` embedded in the page is rendered conditionally, and the embedded page of the <embed-link> is also markdown, so we need a <quake-render> , So you can "mock doll" infinitely, until the current page of the browser is hung up.

In terms of usage, the two components A and B do not have such a mutual calling relationship.

PS: Actually this is a bug. Later I thought it was a features.

### Sidecar pattern

In the cloud-native patterns, the sidecar model refers to the deployment of application components into separate processes or containers to provide isolation and encapsulation. In this regard, it is also very simple for Web Components.

### Other parterns

There are still many, you can play slowly when you have time.

## Summarize

Think outside the frame and think about the problem, and you will find all kinds of very interesting things.
  
  
