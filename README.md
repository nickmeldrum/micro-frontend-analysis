# micro-frontend-analysis

## Research

 * lots of good linked resources there https://medium.com/@tomsoderlund/micro-frontends-a-microservice-approach-to-front-end-web-development-f325ebdadc16
 * single-spa https://single-spa.js.org/
 * hellofresh engineering https://engineering.hellofresh.com/front-end-microservices-at-hellofresh-23978a611b87
 * project mosaic https://www.mosaic9.org
 * Ask HN https://news.ycombinator.com/item?id=13009285
 * list https://github.com/ChristianUlbrich/awesome-microfrontends
 * micro frontends https://micro-frontends.org/
 * SCS https://scs-architecture.org/
 * ROCA https://roca-style.org/
 * Martin Fowler: https://martinfowler.com/articles/micro-frontends.html
 * list of js framework support for custom elements: https://custom-elements-everywhere.com/
 * Thoughtworks technology radar: https://www.thoughtworks.com/radar/techniques/micro-frontends

## Some articles

 * https://blog.pragmatists.com/independent-micro-frontends-with-single-spa-library-a829012dc5be
 * https://hackernoon.com/understanding-micro-frontends-b1c11585a297
 * https://medium.com/@lucamezzalira/i-dont-understand-micro-frontends-88f7304799a9
 * this is the best article so far imo: https://medium.com/@benjamin.d.johnson/exploring-micro-frontends-87a120b3f71c

## What kind of problem are you trying to solve?

independence of different teams? what kind of independence? ability to deploy separately? team a can ship without waiting for team b? then you need separate pipelines. You need to consider testing strategies. You probably need versioning or contract testing in place?

team isolation without separate deployment and perhaps you split components out to packages?

1 model is to have reusable base components built by 1 team - i.e. build a component library with storybook 
perhaps have other component teams/ codebases run by other teams that build feature specific components - with their own backend
lose the value of 1 global state ala redux if you have multiple stores though potentially.
or just the UI part of the feature components deployable - that way

benefits of team isolation:

simplicity of scheduling work
autonomous teams can work more efficiently - not waiting on things outside of their control
requirement to improve testing/ contracts at the boundaries though

benefits of separate deployment?

increased pipeline performance
business agility - deploy 1 thing without the other (ideal world you can always deploy master any time of day, so perhaps only if you are still working towards a perfect CD pipeline?)

## how do you do it?

You will need an orchestration layer:

 * knows how to load each micro-frontend - SSIs or ESIs or ...?
 * knows where to stick them into page - global layouts?

possible separation mechanisms:

 * iFrames
 * web components - not ready for prime time - still discussions on implementation by whatwg etc. see https://developer.mozilla.org/en-US/docs/Web/Web_Components/HTML_Imports
 * custom events/ dom/ namespacing!

## make it consistent

shared components - storybook - company design system

## multi tech stacks?

sure in theory - treat with care though...

it will impact customer performance - more scripts to download
it will impact team resourcing - more to learn

## Why didn't I recommend waitrose moved to a micro-frontend?

What problem were they trying to solve? Mostly it was about teams being able to work independently. However they were not doing full vertical features for a variety of reasons.

1. was that the backend was creating an api surface for multiple existing clients as well as trying to think about making these apis public and usable by potentially anyone. They had to work and make sense in complete isolation from any client/ front end. Therefore there was a strong desire to run backend/ api stories separate from the frontends. We would of course collaborate deeply when designing the APIs/ user journey flows and stories. But they would be collaborating with the web frontenders as *well* as the mobile teams as *well* as other api teams which were also consumers of course.

This meant that stories made sense to be created at the API level. And subsequently (or in parallel) a set of frontend stories would be created.

