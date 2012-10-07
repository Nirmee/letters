**Letters** is a little alphabetical library that makes sophisticated debugging easy &amp; fun.

*Quick note about Rails*: Until I build a Rails-specific gem, I'm changing Letters to patch `Object` by default. To only patch core classes, `require "letters/patch/core"`. For Rails support, `require "letters/patch/rails"`. Make sure to do this after `Bundler.require` in `application.rb`. 

For many of us, troubleshooting begins and ends with the `print` statement. Others recruit the debugger, too. (Maybe you use `print` statements to look at changes over time but the debugger to focus on a small bit of code.) These tools are good, but they are the lowest level of how we can debug in Ruby. Letters leverages `print`, the debugger, control transfer, computer beeps, and other side-effects for more well-rounded visibility into code and state.

### Installation ###

If you're using RubyGems, install Letters with:

    gem install letters

By default, requiring `"letters"` monkey-patches `Object`. It goes without saying that if you're using Letters in an app that has environments, you probably only want to use it in development.

### Debugging with letters ###

With Letters installed, you have a suite of methods available wherever you want them in your code -- at the end of any expression, in the middle of any pipeline. Most of these methods will output some form of information, though there are more sophisticated ones that pass around control of the application.

There are almost 20 Letters methods so far. You can find them [in the documentation](http://lettersrb.com/api).

Let's use with the `o` method as an example. It is one of the most familiar methods. Calling it prints the receiver to STDOUT and returns the receiver:

```ruby
{ foo: "bar" }.o 
# => { foo: "bar" }
# prints { foo: "bar" }
```

That's simple enough, but not really useful. Things get interesting when you're in a pipeline:

```ruby
words.grep(/interesting/).
  map(&:downcase).
  group_by(&:length).
  values_at(5, 10).
  slice(0..2).
  join(", ")
```   

If I want to know the state of your code after lines 3 and 5, all I have to do is add `.o` to each one:

```ruby
words.grep(/interesting/).
  map(&:downcase).
  group_by(&:length).o.
  values_at(5, 10).
  slice(0..2).o.
  join(", ")
```

Because the `o` method (and nearly every Letters method) returns the original object, introducing it is only ever for side effects -- it won't change the output of your code.

This is significantly easier than breaking apart the pipeline using variable assignment or a hefty `tap` block.

The `o` method takes options, too, so you can add a prefix message to the output or choose another output format -- like [YAML]() or [pretty print]().

### The methods ###
<table>
  <tr>
    <th>Letter</th>
    <th>Command</th>
    <th>Options</th>
    <th>Description</th>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#a">a</a>
    </td>
    <td>
      Assert
    </td>
    <td>
      :message, 
      :error_class
    </td>
    <td>
      asserts in the context of its receiver or Letters::AssertionError
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#b">b</a>
    </td>
    <td>
      Beep
    </td>
    <td>
    </td>
    <td>
      causes your terminal to beep
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#c">c</a>
    </td>
    <td>
      Callstack
    </td>
    <td>
      :message 
    </td>
    <td>
      prints the current callstack
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#d">d</a>
    </td>
    <td>
      Debugger
    </td>
    <td>
      
    </td>
    <td>
      passes control to the debugger
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#d1/d2">d1/d2</a>
    </td>
    <td>
      Diff
    </td>
    <td>
      :message,
      :format,
      :stream
    </td>
    <td>
      prints a diff between first and second receivers
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#e">e</a>
    </td>
    <td>
      Empty
    </td>
    <td>
      :message
    </td>
    <td>
      raises a Letters::EmptyError if its receiver is empty
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#f">f</a>
    </td>
    <td>
      File
    </td>
    <td>
      :format, :name
    </td>
    <td>
      writes its receiver into a file in a given format
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#j">j</a>
    </td>
    <td>
      Jump
    </td>
    <td>
      (&block)
    </td>
    <td>
      executes its block in the context of its receiver
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#k">k</a>
    </td>
    <td>
      Kill
    </td>
    <td>
      :max
    </td>
    <td>
      raises Letters::KillError after a maximum number of calls
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#l">l</a>
    </td>
    <td>
      Logger
    </td>
    <td>
      :format, :level
    </td>
    <td>
      logs its receivers on the available logger instance
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#m">m</a>
    </td>
    <td>
      Mark as tainted
    </td>
    <td>
      (true|false)
    </td>
    <td>
      taints (or untaints) its receiver
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#n">n</a>
    </td>
    <td>
      Nil
    </td>
    <td>
      
    </td>
    <td>
      raises a Letters::NilError if its receiver is nil
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#o">o</a>
    </td>
    <td>
      Output
    </td>
    <td>
      :format,
      :stream
    </td>
    <td>
      prints its receiver to standard output
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#r">r</a>
    </td>
    <td>
      Ri
    </td>
    <td>
      (method name as symbol)
    </td>
    <td>
      prints RI documentation of its receiver class
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#s">s</a>
    </td>
    <td>
      Safety
    </td>
    <td>
      (level number)
    </td>
    <td>
      bumps the safety level (by one or as specified)
    </td>
  </tr>

  <tr>
    <td>
      <a href="http://lettersrb.com/api#t">t</a>
    </td>
    <td>
      Timestamp
    </td>
    <td>
      :time_format
    </td>
    <td>
      prints out the current timestamp
    </td>
  </tr>
</table>

See the [full documentation](http://lettersrb.com/api) for examples and more detailed explanations.

### Configuration ###

For maximum productivity, you can tune and tweak each Letters method to fit your own tastes. Want to name put files somewhere else? No problem. Don't like YAML? Default `f` to use Pretty Print instead! The world of defaults is your oyster.

```ruby
Letters.config do
  f :format => "pp", :name => "my-special-file"
end
```
