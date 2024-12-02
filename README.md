# Unity_Language_Select
유니티 C# 언어변경

## 유니티에서 여러 언어에 대한 설정을 하고싶을때 사용한 방법
### 1
    using System.Collections.Generic;
    using TMPro;
    using UnityEngine;

    public enum EnumLanguage { eNULL, eKOREAN, eENGLISH };

    public class Instance
    {
        public static EnumLanguage eLanguage = EnumLanguage.eNULL;
    }

    public class LanguageManager : MonoBehaviour
    {
        public static LanguageManager S;
        public TMP_Dropdown languageDropdown;
        private List<string> optionList = new List<string>();
        public string returnWord;
        private void Awake()
        {
            S = this;

            string language = PlayerPrefs.GetString("Language");
            //Debug.Log($"현재 {language} 입니다. ");
            optionList.Add("한국어");
            optionList.Add("ENGLISH");
            languageDropdown.AddOptions(optionList);
            if (language.Equals("")) //기본값 한국어
            {
                // Instance.eLanguage = EnumLanguage.eENGLISH;
                // SettingLanguage(Instance.eLanguage);
                // languageDropdown.value = 2;
                Instance.eLanguage = EnumLanguage.eKOREAN;
                SettingLanguage(Instance.eLanguage);
                languageDropdown.value = 1;
            }
            else if (language.Equals("KOREAN"))
            {
                Instance.eLanguage = EnumLanguage.eKOREAN;
                languageDropdown.value = 0;
            }
            else if (language.Equals("ENGLISH"))
            {
                Instance.eLanguage = EnumLanguage.eENGLISH;
                languageDropdown.value = 1;
            }
            else
            {
            }
        }

        private void Start()
        {
            languageDropdown.onValueChanged.AddListener(delegate { SetDropDown(); });
        }

        private void SetDropDown()
        {
            string selectedOption = languageDropdown.options[languageDropdown.value].text;
            Debug.Log(selectedOption);
            if (selectedOption == "한국어")
            {
                SettingLanguage(EnumLanguage.eKOREAN);
            }
            else if (selectedOption == "ENGLISH")
            {
                SettingLanguage(EnumLanguage.eENGLISH);
            }
            TextChangerEventManager.TriggerApplyLanguage();
        }

        private void SettingLanguage(EnumLanguage language)
        {
            optionList.Clear();
            if (language == EnumLanguage.eKORE출
            return returnWord;
        }
    }


### 2 
    using System;
    using TMPro;
    using UnityEngine;
    using UnityEngine.UI;

    public class TextChanger : MonoBehaviour
    {
        [SerializeField]
        private string wordForDebug;
    
        private void Start()
        {
            ChangeLanguage();
        }
    
        private void OnEnable()
        {
            TextChangerEventManager.OnLanguageChangedEvent += ChangeLanguage;
        }
    
        private void OnDisable()
        {
            TextChangerEventManager.OnLanguageChangedEvent -= ChangeLanguage;
        }
    
        private void ChangeLanguage()
        {
            // PlayerPref에 저장된 오브젝트 이름에 대응하는 텍스트 획득
            wordForDebug = LanguageManager.S.ReturnWord(transform.gameObject.name);
    
            if (wordForDebug != "")
            {
                Text textComponent = GetComponent<Text>();
                if (textComponent != null)
                {
                    textComponent.text = wordForDebug;
                }
    
                TextMeshProUGUI tmpUguiComponent = GetComponent<TextMeshProUGUI>();
                if (tmpUguiComponent != null)
                {
                    tmpUguiComponent.text = wordForDebug;
                }
    
                TextMeshPro tmpComponent = GetComponent<TextMeshPro>();
                if (tmpComponent != null)
                {
                    tmpComponent.text = wordForDebug;
                }
            }
        }
    }


    public static class TextChangerEventManager
    {
        public static event Action OnLanguageChangedEvent;

        public static void TriggerApplyLanguage()
        {
            OnLanguageChangedEvent?.Invoke();
        }
    }

