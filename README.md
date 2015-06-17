# RetroFacebook

![RetroFacebook.png](art/retrofacebook.png)

OGM, Object Graph Mapping, Graph to POJO. Support V3 and V4.

Inspired by retrofit.


![photos.png](art/screenshot-photos.png)
![posts.png](art/screenshot-posts.png)

## Usage

```java
Facebook facebook = Facebook.create(activity);

Observable<Post> posts = facebook.getPosts("4");
posts.subscribe(post -> System.out.println(post.id()));

Observable<Post> myPosts = facebook.getPosts();
myPosts.subscribe(post -> System.out.println(post.id()));

Observable<Photo> myPhotos = facebook.getPhotos();
myPhotos.subscribe(photo -> System.out.println(photo.id()));
```

[retrofacebook/src/main/java/retrofacebook/Facebook.java](retrofacebook/src/main/java/retrofacebook/Facebook.java):

```java
@RetroFacebook
abstract class Facebook {
    @RetroFacebook.GET("/{user-id}")
    Observable<Post> getPosts(@RetroFacebook.Path("user-id") String userId);

    @RetroFacebook.GET("/{user-id}")
    Observable<Photo> getPhotos(@RetroFacebook.Path("user-id") String userId);

    // ...
}
```

[retrofacebook/src/main/java/retrofacebook/Post.java](retrofacebook/src/main/java/retrofacebook/Post.java):

```java
@AutoJson
public abstract class Post {
    @Nullable
    @AutoJson.Field
    public abstract String id();

    @Nullable
    @AutoJson.Field(name = "is_hidden")
    public abstract Boolean isHidden();

    // ...
}
```

## Installation

via jitpack.io:

```
repositories {
    maven {
        url "https://jitpack.io"
    }
}

dependencies {
    compile 'com.github.yongjhih:RetroFacebook:1.0.1'
}
```

## Development

[retrofacebook/src/main/java/retrofacebook/Facebook.java](retrofacebook/src/main/java/retrofacebook/Facebook.java):
[retrofacebook/src/main/java/retrofacebook/Post.java](retrofacebook/src/main/java/retrofacebook/Post.java):

Generated(v4):

```java
final class RetroFacebook_Facebook extends Facebook {

  @Override
  public Observable<Post> getPost(java.lang.String postId) {
        return Observable.create(new OnSubscribeGraphResponse(
                GraphRequest.newGraphPathRequest(
                    AccessToken.getCurrentAccessToken(),
                    "/" + postId + "", null
                )
            )
        ).map(new Func1<GraphResponse, Post>() {
            @Override public Post call(GraphResponse response) {
                try {
                    return LoganSquare.parse(response.getJSONObject().toString(), AutoJson_Post.class);
                } catch (java.io.IOException e) {
                    e.printStackTrace();
                    return null;
                }
            }
        });
  }

  //...

}
```

```java
@JsonObject
final class AutoJson_Post extends Post {

  @android.support.annotation.Nullable
  @com.bluelinelabs.logansquare.annotation.JsonField
  String id;

  @android.support.annotation.Nullable
  @com.bluelinelabs.logansquare.annotation.JsonField(name = {"is_hidden"})
  Boolean isHidden;

  public AutoJson_Post() {
    super();
  }

  private AutoJson_Post(
      String id,
      Boolean isHidden) {
    this.id = id;
    this.isHidden = isHidden;
  }

  @Override
  public String id() {
    return id;
  }

  @Override
  public Boolean isHidden() {
    return isHidden;
  }

  @Override
  public String toString() {
    return "Post{"
        + "id=" + id + ", "
        + "isHidden=" + isHidden
        + "}";
  }

  @Override
  public boolean equals(Object o) {
    if (o == this) {
      return true;
    }
    if (o instanceof Post) {
      Post that = (Post) o;
      return ((this.id == null) ? (that.id() == null) : this.id.equals(that.id()))
           && ((this.isHidden == null) ? (that.isHidden() == null) : this.isHidden.equals(that.isHidden()));
    }
    return false;
  }

  // ...
```

## Credit

* https://github.com/frankiesardo/auto-parcel
* https://github.com/bluelinelabs/LoganSquare

## License

```
Copyright 2015 8tory, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
