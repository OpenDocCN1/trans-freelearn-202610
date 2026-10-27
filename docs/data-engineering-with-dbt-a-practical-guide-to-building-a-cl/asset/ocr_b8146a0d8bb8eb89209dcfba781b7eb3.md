By chaining an STG model, a HIST model, and the current_from_history macro, you now have all the pieces of the puzzle to build a simple, highly resilient load pipeline that allows you to adjust, store, and serve the full range of incoming data for business use in an efficient and highly scalable way.

In the last chapter, we will look at patterns for ingesting data and storing history with more advanced needs such as tracking deletion and multiple changes per load run.

But before that, in the next chapter, we will use our newfound knowledge to refactor our sample project and use the save_history macro to create HIST models, add more data, and implement some business rules.

# Summary

In this chapter, you learned how to build modular pipelines based on well-thought-out keys that reflect the desired semantics of entities.

You learned how to combine entities from different systems and how to store their changes simply and effectively, according to their semantics.

With the knowledge you’ve gained in this chapter, you are now able to build modern, maintainable, and scalable data platforms with hundreds of entities and hundreds of millions, or even billions, of rows in the major tables.

In the next chapter, we will wrap up our sample project and use it to discuss a few more topics that come in handy when delivering a real-life project.