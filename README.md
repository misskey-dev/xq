# XQ (WIP)
Make federation better!

XQ (pronounced as "clock") is a high-efficiency federation protocol for microblogging.

Protocols such as ActivityPub are widely used and useful, but unfortunately are not the best option when efficiency is important.
Messages are in plain JSON format, which is wasteful, and extensions by various implementations complicate the implementation.

XQ aims to solve those problems through the following features:

- Using Protocol Buffers, which are in binary format, allows communication to be in a format with a fully pre-defined schema, making the message as small as possible.
  - Having schema definitions available in a variety of languages is also expected to facilitate application development.
- Statically-typed-language-friendly structure.
- Instead of user-specific signatures, server-specific signatures will be used.
  - There is no security benefit to signing per user, so reviewing this will improve performance. The signing process constitutes a significant portion of the federation.
- Eliminate unnecessary data and boilerplate by focusing on microblogging for its intended use.
  - If it proves to be scalable for other purposes without compromising efficiency, it may be possible to support use cases other than microblogging.
- Allows multiple messages to be combined into a single request to reduce overhead.

**Note that this protocol is intended to be used primarily for Misskey-to-Misskey communication for efficiency, but any software with a similar concept to Misskey should be able to use it.**

This repository contains specification documentation and .proto definitions.

## FAQ

### Why was this protocol created?

Thinking up protocols is fun, isn't it?

By reinventing the wheel, we not only gain a better understanding of existing technology, but we also have the potential to make a better wheel than the existing wheel.
It is through this creative mindset that Misskey was born.

It is true that there is room for improvement in existing protocols.

### The performance benefits of using Protobuf don't seem significant.

As you can see from this README, performance is not the only objective.
We also emphasize ease of application development.

Also, even if the benefits are “not that significant,” if they are even a little bit significant, it is worth adopting.

### Will it be used in Misskey?

Not at this time.
Just because we have created a protocol for Misskey does not mean that it must be used with Misskey.

### If requests are processed in batches, won't real-time performance be lost?

Yes. Depending on the interval of batch processing, real-time performance may be lost.
Since activity bundling is optional in XQ, it is possible to bundle only those messages for which real-time is not critical.

Note that existing protocols are not necessarily more real-time efficient.
Typical implementations use job queues, and there can be some lag in processing jobs.
