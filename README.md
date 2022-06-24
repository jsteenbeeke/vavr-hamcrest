vavr-hamcrest
=============

[![License](https://img.shields.io/github/license/jsteenbeeke/vavr-hamcrest)](http://www.gnu.org/licenses/lgpl-3.0.html)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.jeroensteenbeeke.vavr/vavr-hamcrest/badge.svg?style=flat-square)](http://search.maven.org/#search|gav|1|g:"com.jeroensteenbeeke.vavr"%20AND%20a:"vavr-hamcrest")

A set of Hamcrest matchers to use with the [vavr](https://vavr.io) library.

## Install

Add the matchers as a test dependency to your project.

#### Maven
```xml
<dependencies>
	<dependency>
		<groupId>com.jeroensteenbeeke.vavr</groupId>
		<artifactId>vavr-hamcrest</artifactId>
		<version>1.0.0</version>
		<scope>test</scope>
	</dependency>
</dependencies>
```

#### Gradle
```groovy
dependencies {
	testImplementation 'com.jeroensteenbeeke.vavr:vavr-hamcrest:1.0.0'
}
```

## Usage

Using these matchers is no different from using other Hamcrest matchers.

```java

import io.vavr.control.Either;
import io.vavr.control.Option;
import io.vavr.control.Try;
import org.junit.jupiter.api.Test;

import static com.jeroensteenbeeke.vavr.hamcrest.VavrMatchers.*;
import static org.hamcrest.MatcherAssert.assertThat;

public class ExampleTest {
	@Test
	void examples() {
		// Option
		assertThat(Option.none(), isNone());
		assertThat(Option.none(), isEmptyOption());
		assertThat(Option.some("A"), isSome());
		assertThat(Option.some("A"), isSome("A"));
		assertThat(Option.some("A"), isDefinedOption());
		assertThat(Option.some("A"), isDefinedOption("A"));
		assertThat(Option.some("A"), isDefinedOption("contains only alphabetic letters", s -> s.matches("^[a-zA-Z]*$")));

		// Either
		assertThat(Either.left("Error"), isLeft());
		assertThat(Either.left("Error"), isLeft("Error"));
		assertThat(Either.left("Error"), isLeft("Starts with an E", s -> s.startsWith("E")));
		assertThat(Either.right("Success"), isRight());
		assertThat(Either.right("Success"), isRight("Success"));
		assertThat(Either.right("Success"), isRight("Starts with an S", s -> s.startsWith("S")));

		// Try
		assertThat(Try.success("S"), isSuccess());
		assertThat(Try.success("S"), isSuccess("S"));
		assertThat(Try.success("S"), isSuccess("Starts with an S", v -> v.startsWith("S")));

		assertThat(Try.failure(new RuntimeException()), isFailure());
		assertThat(Try.failure(new IllegalStateException()), isFailure(IllegalStateException.class));
		assertThat(Try.failure(new IllegalArgumentException()), isFailure("Is a runtime exception", t -> t instanceof RuntimeException));

	}
}


```
