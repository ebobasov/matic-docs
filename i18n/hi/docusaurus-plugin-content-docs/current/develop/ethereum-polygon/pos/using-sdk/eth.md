---
id: eth
title: एथ डिपाज़िट करने और निकालने की गाइड
sidebar_label: ETH
description: "पॉलीगॉन नेटवर्क पर एथ टोकन डिपाज़िट करें और निकालें."
keywords:
  - docs
  - matic
  - ether
  - withdraw
  - deposit
image: https://matic.network/banners/matic-network-16x9.png
---

[एथ पर लेटेस्ट Matic.js डॉक्यूमेंटेशन](https://maticnetwork.github.io/matic.js/docs/pos/deposit-ether/) देखें.

## झटपट वाला सारांश {#quick-summary}

डॉक्यूमेंट का यह हिस्सा इसके संबंध में है कि पॉलीगॉन नेटवर्क पर erc20 टोकन कैसे डिपाज़िट करें और निकालें. एथ, erc20, erc721 और erc1155 के डॉक्यूमेंट के हिस्सों में समान फ़ंक्शन मौजूद हैं, जिनमें नामकरण और लागू करने के पैटर्न में भिन्नताएँ होती हैं जो उस मापदण्ड के अनुरूप होती हैं. डॉक्यूमेंट के इस हिस्से का इस्तेमाल करने के लिए सबसे महत्वपूर्ण शर्त आपके असेट की मैपिंग करना है, इसलिए कृपया मैपिंग का अनुरोध [यहाँ](https://docs.polygon.technology/docs/develop/ethereum-polygon/submit-mapping-request/) सबमिट करें.

## भूमिका {#introduction}

यह गाइड पॉलीगॉन टेस्टनेट (Mumbai) का इस्तेमाल करती है जो दो ब्लॉकचेन के बीच असेट ट्रांसफ़र का प्रदर्शन करने के लिए अपने आप में ही Goerli नेटवर्क में मैप किया हुआ है. यह नोट करना महत्वपूर्ण है कि ट्यूटोरियल के उद्देश्यों के लिए, जब भी संभव हो तो आपको प्रॉक्सी पते का इस्तेमाल करना चाहिए. यह इसलिए है क्योंकि जब अनुबंध कोड में कोई नया अपडेट जोड़ा जाता है तब लागू करने के अनुबंध पते में बदलाव संभव है, प्रॉक्सी कभी नहीं बदलता और यह आने वाले सभी अनुरोधों को सबसे नए लागू हुए स्थान पर भेज देता है. वास्तव में, अगर आप प्रॉक्सी पते का इस्तेमाल करते हैं, तो आपको लागू हुए अनुबंध पर होने वाले किसी भी बदलाव की चिंता करने की ज़रूरत नहीं होगी.

उदाहरण के लिए, पते के बजाय बातचीत के लिए `RootChainManagerProxy`पता का इस्तेमाल `RootChainManager`करें. PoS कॉन्ट्रैक्ट पता, ABI और टेस्ट टोकन के पते जैसे डिप्लॉयमेंट विवरण [यहां](/docs/develop/ethereum-polygon/pos/deployment/) पाए जा सकते हैं.

पॉस ब्रिज को ऐप्लिकेशन पर इंटीग्रेट करने के लिए अपने असेट को मैप करना एक ज़रूरी स्टेप है, इसलिए अगर आपने यह नहीं किया है, तो अपना मैपिंग अनुरोध [यहाँ](https://docs.polygon.technology/docs/develop/ethereum-polygon/submit-mapping-request/) सबमिट करें. इस ट्यूटोरियल के उद्देश्यों के लिए, टीम ने टेस्ट टोकन डिप्लॉय किए हैं और उन्हें पॉस ब्रिजसे मैप कर दिया है. आप उस असेट का अनुरोध [फ़ॉसेट](https://faucet.polygon.technology/) पर करें जिसे आप इस्तेमाल करना चाहते हैं और अगर टेस्ट टोकन अनुपलब्ध हैं, तो टीम से [डिस्कॉर्ड](https://discord.com/invite/0xPolygon) पर संपर्क करें. हम आपको तुरंत जवाब देना सुनिश्चित करेंगे.

आगे आने वाले ट्यूटोरियल में, कोड के चंद छोटे-छोटे हिस्सों के साथ-साथ हर स्टेप को विस्तार में समझाया जाएगा. हालाँकि, आप कभी भी [रिपाज़िटोरी](https://github.com/maticnetwork/matic.js/tree/master/examples) से मदद ले सकते हैं जहाँ सभी **उदाहरण सोर्स कोड** होंगे, जो आपको पॉस ब्रिज को इंटीग्रेट करने और उसके काम करने के तरीके को समझने में सहायता कर सकते हैं.

## उच्च स्तरीय फ़्लो {#high-level-flow}

एथ डिपाज़िट करें -

1. **_इनके लिए एथेर डिपाज़िट करें_** के लिए **_रूट चेन मैनेजर_** का सहारा लें और आवश्यक एथेर **भेजें **.

एथ निकालें -

1. पॉलीगॉन चेन पर टोकन **_बर्न करें_**.
2. बर्न ट्रांज़ैक्शन का सबूत सबमिट करने के लिए **_रूट चेन मैनेजर_** पर **_बाहर निकलें_** फ़ंक्शन का सहारा लें. इसका सहारा तभी लिया जा सकता है जब बर्न ट्रांज़ैक्शन वाले ब्लॉक के लिए **_चेकपॉइंट_** सबमिट कर दिया गया हो.

## स्टेप्स {#steps}

### डिपाज़िट करें {#deposit}

**रूट चेन मैनेजर** अनुबंध पर **इनके लिए एथेर डिपाज़िट करें** का सहारा लेकर एथ पॉलीगॉन चेन में जमा किए जा सकते हैं. इसका सहारा लेने के लिए पॉलीगॉन पॉस क्लाइंट **एथेर डिज़िट** करें तरीके को पेश करता है.

```jsx
const result = await posClient.depositEther(<amount>);
const txHash = await result.getTransactionHash();
const txReceipt = await result.getReceipt();
```

:::note
Ethereum से पॉलीगॉन में जमा हो जाता है और स्टेट **सिंक** मैकेनिज्म का इस्तेमाल करके यह लगभग 22-30 मिनट का समय लेता है. इस बार के अंतराल का इंतजार करने के बाद, यह सिफारिश की जाती है कि वेब3.js/mattic.js लाइब्रेरी का इस्तेमाल करके संतुलन की जांच करें या Metamask. का इस्तेमाल करें. एक्सप्लोरर तभी बैलेंस दिखाएगा जब चाइल्ड चेन पर कम से कम एक असेट ट्रांसफ़र हुआ हो. यह [<ins>लिंक</ins>](/docs/develop/ethereum-polygon/pos/deposit-withdraw-event-pos/) डिपोजिट इवेंट को ट्रैक करने के लिए बताता है.
:::

### बर्न करें {#burn}

ET, पॉलीगॉन चेन पर ERC20 टोकन के रूप में जमा किया जाता है. वापस लेना ERC20 टोकन को वापस लेने के रूप में उसी प्रक्रिया का अनुसरण करता है.

टोकन को जलाने और निकासी की प्रक्रिया को शामिल करने के लिए, MaticWETH कॉन्ट्रैक्ट के the स को कॉल करें. चूंकि ईथर पॉलीगॉन चेन पर ERC20 टोकन है, इसलिए आपको पॉलीगॉन PoS क्लाइंट से **ERC20** टोकन शुरू करने की need the होती है और फिर बर्न की प्रक्रिया शुरू करने के लिए `withdrawStart()`विधि को कॉल करना पड़ता है.

```jsx
const erc20Token = posClient.erc20(<token address>);

// start withdraw process for 100 amount
const result = await erc20Token.withdrawStart(100);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```

इस कॉल के लिए ट्रांज़ैक्शन हैश स्टोर करें और बर्न का सबूत जनरेट करते समय इसका इस्तेमाल करें.

### बाहर निकलें {#exit}


एक बार **जब की चेकपॉइंट** को बर्न ट्रांसक्शन वाले ब्लॉक के लिए पेश किया गया है, तो यूजर को `RootChainManager`कॉन्ट्रैक्ट के **एग्जिट** फंक्शन को कॉल करना चाहिए और बर्न का सबूत पेश करना चाहिए. मान्य सबूत सबमिट हो जाने पर, यूज़र को टोकन ट्रांसफ़र कर दिए जाते हैं. पॉलीगॉन पॉस क्लाइंट इसका सहारा लेने के लिए `withdrawExit` तरीके को `erc20`पेश करता है. इस फ़ंक्शन का सहारा तभी लिया जा सकता है जब मुख्य चेन में चेकपॉइंट शामिल किया गया हो. इस [गाइड](/docs/develop/ethereum-polygon/pos/deposit-withdraw-event-pos.md#checkpoint-events) को फ़ॉलो करके चेकपॉइंट के शामिल होने को ट्रैक किया जा सकता है.


```jsx
// token address can be null for native tokens like ethereum or matic
const erc20RootToken = posClient.erc20(<token address>, true);

const result = await erc20Token.withdrawExit(<burn tx hash>);

const txHash = await result.getTransactionHash();

const txReceipt = await result.getReceipt();

```
