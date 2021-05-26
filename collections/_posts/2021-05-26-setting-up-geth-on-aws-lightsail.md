---
title: "Simple guide to Geth on AWS Lightsail"
description: "A 102 on running your own nodes with cost, failover and security considerations."
categories: [guides]
tags: [ethereum, geth, aws, lightsail, staking]
last_modified_at: "2021-05-26"
published: false
---

I've been thinking about how we can truly have a decentralized internet.

You are either one of three levels of expertise when it comes to running your own node:
- L1: Your everyday Jamie who is more like to delegate staking on third-party platforms
- L2: You're _in the tech space_, and can probably do some infrastructure administration
- L3: The experts. You know what you're doing and probably have been runnning your own machines since the dot-com boom.

## Getting Started


Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus tempor velit ipsum, nec molestie elit volutpat ac. Ut vel feugiat mauris, at pretium leo. Quisque volutpat pulvinar eros vitae tincidunt. Morbi et urna in felis egestas convallis quis sed metus. Quisque nec malesuada urna. Donec consequat nibh velit, et faucibus mauris venenatis in. Suspendisse mollis, erat ac pellentesque cursus, lacus augue bibendum sapien, ut maximus diam arcu at tortor. Etiam cursus leo eu sapien sagittis porttitor. Pellentesque consectetur nisi quis felis porttitor, et suscipit risus semper. Cras convallis neque ut ullamcorper pellentesque. Nulla magna lectus, molestie nec ante sed, sollicitudin tempus urna. Vestibulum condimentum rutrum purus, vitae consequat justo rhoncus eu.

In a arcu vitae nulla tempus lacinia. Phasellus pellentesque erat faucibus urna vulputate vulputate. Maecenas porttitor mauris sed est ultricies, vel euismod orci interdum. Cras tristique in eros vitae suscipit. Vestibulum a neque ex. Sed eget eros ac tellus euismod auctor ut ut augue. Aliquam erat volutpat. Nullam mauris urna, auctor id feugiat vitae, mollis non massa. Integer pretium pretium nibh quis elementum. Fusce gravida metus vitae sem tincidunt gravida. Etiam non ultricies neque. Pellentesque eget rutrum nulla.

Cras lacinia, tellus id condimentum hendrerit, nibh neque molestie felis, in fringilla magna sapien eget lorem. Suspendisse volutpat ullamcorper egestas. Fusce finibus feugiat mi, a dictum elit egestas scelerisque. Pellentesque lectus eros, consectetur suscipit lobortis vel, fringilla vel sem. Etiam feugiat, elit non euismod rhoncus, risus nibh auctor tellus, in vulputate lacus diam in ipsum. Praesent nec consectetur arcu. Aliquam sodales nunc quis metus posuere imperdiet. Aliquam iaculis nulla sit amet augue vulputate, vel finibus dui semper. Proin est dolor, dignissim quis elit at, imperdiet ornare purus. Phasellus vitae venenatis diam. Pellentesque ac sagittis ligula.

## Security

Cras tempus pretium libero. Nam nec egestas diam. Nullam ut sodales nisi. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Maecenas pulvinar dui in ligula vulputate, a malesuada est facilisis. Vivamus a finibus nibh, ut tempus ipsum. Pellentesque condimentum magna felis, eu ultrices enim iaculis id. Aliquam nec nisl vestibulum, ornare est in, euismod leo. Duis feugiat magna congue ex malesuada, vestibulum pellentesque quam cursus. Maecenas a commodo sem. Vestibulum maximus vestibulum justo in faucibus. Donec eu placerat risus. Mauris ullamcorper, lacus ac ornare tincidunt, ante orci euismod lorem, eu posuere orci purus sed mi.

Aenean eget ex blandit, placerat augue pellentesque, congue urna. Phasellus congue nibh eget massa maximus, sit amet pharetra risus congue. Nullam lectus massa, congue quis varius a, hendrerit vitae mauris. Vivamus eu justo quam. Nam risus eros, mattis ut rhoncus sed, ullamcorper fermentum neque. Aenean et pulvinar tellus, vitae vulputate diam. Aenean laoreet mattis dui, eu lacinia libero egestas in. Suspendisse id mi iaculis, luctus mauris nec, scelerisque diam. Sed at tortor vitae mi fermentum dignissim.


## Cost Estimation


Vestibulum ullamcorper risus pharetra, porttitor velit ut, aliquet augue. Nunc malesuada molestie ante sed viverra. Aliquam porttitor bibendum sollicitudin. Donec sit amet diam sit amet erat aliquam dictum eget vitae ligula. Sed eget placerat arcu. Aliquam accumsan sapien in enim dictum, vitae convallis neque eleifend. Donec pulvinar est vitae felis porta rhoncus. Praesent vel lobortis nulla. Maecenas eu elementum tellus, ut vestibulum eros. Morbi et magna imperdiet, congue dui sit amet, malesuada urna. Cras dictum tellus gravida nulla convallis tempus. Nullam ac ligula ipsum. Fusce et risus ac sem euismod lacinia. Quisque id magna commodo, ultrices turpis porta, viverra mauris.

Nunc odio metus, sollicitudin sed elementum sit amet, ornare vel quam. Donec nec accumsan lectus. Aliquam mauris enim, imperdiet vitae mi ut, fringilla sagittis sem. Proin ipsum eros, convallis a ex id, cursus viverra arcu. Cras lobortis commodo aliquam. In commodo mattis nunc at maximus. Suspendisse viverra dapibus dui. Maecenas tincidunt nunc non dui sollicitudin, id pellentesque purus sagittis. Suspendisse eu pellentesque mi. Proin euismod eros et metus tempor, non scelerisque lorem convallis. Pellentesque non ultricies lorem. Maecenas iaculis ullamcorper fringilla. Ut non dolor et nisl sodales ultricies nec at erat. Donec ac consequat ligula.


Vestibulum augue mi, malesuada eu suscipit eget, pharetra pretium tortor. Etiam congue bibendum neque eu tincidunt. Praesent aliquet eros finibus finibus faucibus. Pellentesque eu erat sed felis suscipit rhoncus eu eu lacus. Cras ut leo id purus feugiat semper venenatis at eros. Aliquam ultricies massa ut velit mattis, non pharetra lectus congue. Cras feugiat massa non eros suscipit, vitae ultricies tortor aliquam. Sed sit amet diam congue, suscipit ligula vel, ornare orci. Donec quis tincidunt lorem. Duis sed lacus in urna ultricies ornare. Duis blandit euismod nulla, et dictum nibh aliquet id. Fusce auctor accumsan molestie. Phasellus non ultrices tellus. Sed dui enim, porttitor non commodo ut, condimentum eget ex. Donec velit nibh, euismod ornare condimentum nec, euismod ullamcorper quam. In ipsum lorem, egestas non tortor in, maximus efficitur justo.