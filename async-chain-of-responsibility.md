# 异步责任链模型

```java
import java.util.Collections;
import java.util.List;
import java.util.concurrent.CompletionStage;
import java.util.concurrent.Future;

/**
 * 处理请求、返回响应、执行处理链路
 * @author hujia
 */
public class Exchange {
    
    private final Chain chain;
    private final IRouter router;

    public Exchange(List<IInterceptor> interceptors, IRouter router) {
        this.chain = new Chain(interceptors);
        this.router = router;
    }

    public <I extends IRequest, O extends IResponse & CompletionStage<O> & Future<O>> void exchange(I request, O response) {
        try {
            chain.before(request);
            if (router.isAsync(request)) {
                router.async(request, response);
            } else {
                router.sync(request, response);
                response.toCompletableFuture().complete(response);
            }
        } catch (Exception e) {
        }
    }

    static class Chain implements IInterceptor {

        private final List<IInterceptor> interceptors;

        public Chain(List<IInterceptor> interceptors) {
            this.interceptors = Collections.unmodifiableList(interceptors);
        }

        @Override
        public void before(IRequest request) throws Exception {
            for (IInterceptor interceptor : interceptors) {
                interceptor.before(request);
            }
        }

        @Override
        public void after(IRequest request, IResponse response) throws Exception {
            for(var iterator = interceptors.listIterator(interceptors.size()); iterator.hasPrevious(); ) {
                iterator.previous().after(request, response);
            }
        }
    }

    public interface IRouter {

        boolean isAsync(IRequest request);

        void sync(IRequest request, IResponse response);

        <O extends IResponse & CompletionStage<O>> void async(IRequest request, O response);
    }

    public interface IInterceptor {

        default void before(IRequest request) throws Exception {}

        default void after(IRequest request, IResponse response) throws Exception {}
    }

    public interface IRequest {

        int getId();

        int getMsgCode();

        byte[] getContent();
    }

    public interface IResponse {

        void setCode(int code);

        void setContent(byte[] content);
    }
}
```
