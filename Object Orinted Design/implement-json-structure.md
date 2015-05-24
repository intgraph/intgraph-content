[Metadata]
title: Implement a Json Structure Encoder
date: 2015-05-24 15:12:53 

[Tags]
difficulty: 3.5
categories: Type hierarchy
source: facebook

[Description]

Implement a json encoder.

[Solution]

At the very beginning, we have to figure out what is a json or what is in json.

There are some basic type here in the picture.

* Scalar type (int, double)
* String type
* Array
* Dict

![Alt text](http://wizmann-pic.qiniudn.com/3a405bf3e32b2925e4ae027c879ae978)

And after all, all these elements are set in a large dict, and this dict is our json string.

We can represent json structure with a yacc grammar. (Maybe not a precise yacc grammar actually.)

```
json : json_dict

json_key: json_string

json_dict: json_dict "," json_node
         | json_node

json_node: json_key ":" json_value

json_array: json_array "," json_value
          | json_value

json_value: json_scalar
          | json_string
          | json_array
          | json_dict

```

Here, it's easy for us to find out that a json structure is definitely a tree. We have to design a type hierarchy to represent that tree, and find out a way of traversal.

First of all, a pure virtual class as the superclass is essential. Because we can use a pointer of superclass for every subclass of different type.

```cpp
class JsonObject {
public:
    virtual string str() = 0;
};
```

And then, scalar type and string type are easy to deal with.

```cpp
template <typename T>
class JsonScalar: public JsonObject {
public:
    JsonScalar(T value): _value(value) {}
    virtual string str() {
        return to_string(_value);
    }
private:
    T _value;
};

class JsonString: public JsonObject {
public:
    JsonString(const string& str): _str(str) {}
    virtual string str() {
        return "\"" + _str + "\"";
    }
private:
    string _str;
};
```

And then an array type, which contain a vector of `JsonObject*` to store the data of simple type or nested json structure.

```cpp
class JsonArray: public JsonObject {
public:
    JsonArray() {}
    JsonArray(const vector<JsonObject*> vec): _vec(vec) {}
    virtual string str() {
        string res = "[";
        int cnt = 0;
        for (auto p: _vec) {
            if (cnt) {
                res += ", ";
            }
            res += p->str();
            cnt++;
        }
        res += "]";
        return res;
    }
    void add(JsonObject* object_p) {
        _vec.push_back(object_p);
    }
private:
    vector<JsonObject*> _vec;
};
```
And here comes the most important one, the dict class. And it's quite similar with the array type. Of course, it's actually the same.

```cpp
class JsonDict: public JsonObject {
public:
    JsonDict() {}
    JsonDict(const unordered_map<string, JsonObject*> mp): _mp(mp) {}
    virtual string str() {
        string res = "{";
        int cnt = 0;
        for (auto pp: _mp) {
            if (cnt) {
                res += ", ";
            }
            auto key = pp.first;
            auto value = pp.second;
            res += "\"" + key + "\"";
            res += ": ";
            res += value->str();
            cnt++;
        }
        res += "}";
        return res;
    }

    void add(const string& key, JsonObject* value) {
        _mp[key] = value;
    }
private:
    unordered_map<string, JsonObject*> _mp;
};
```

At the very last, we use a `typedef` to implement to root node of the json structure.


```cpp
typedef JsonDict Json;
```

In our main function, we can add everything into a json structure, and print it out to console as a legal json string.

```cpp
int main() {
    // if you want something more insane, you can deal with
    // the whole thing with tons of brackets. Thanks to C++11.
    // LOL
    Json json;
    json.add("winfo_id", new JsonScalar<int>(1));
    json.add("unit_id",  new JsonScalar<int>(2));
    json.add("plan_id",  new JsonScalar<int>(3));
    json.add("user_id",  new JsonScalar<int>(4));
    json.add("url", new JsonString("i am not a url"));
    json.add("bid", new JsonArray({
        new JsonScalar<double>(1.1),
        new JsonScalar<double>(1.2),
        new JsonString("nineteen")
    }));
    json.add("wiseapp", new JsonDict({
        {"Android", new JsonString("url for android")},
        {"iOS",     new JsonString("url for ios")},
        {"WP",      new JsonString("404 not found")}
    }));

    print(json.str());

    return 0;
}
```

Be ware, I haven't implemented the memory recycle because it's just a demo code. When we use it in a production environment, it's easy to deal with the memory allocate and deallocate by a memory pool which is safe, fast and convenient.
