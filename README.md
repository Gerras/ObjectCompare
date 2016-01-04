# ObjectCompare
For testing


    public static class ObjectCompare
    {
        public static bool PublicInstancePropertiesEqual<T>(T self, T to, out string failedVal, params string[] ignore) where T : class
        {
            if (self != null && to != null)
            {
                var type = typeof(T);
                var ignoreList = new List<string>(ignore);
                foreach (var propertyInfo in type.GetProperties(BindingFlags.Public | BindingFlags.Instance))
                {
                    if (!ignoreList.Contains(propertyInfo.Name))
                    {
                        var selfValue = type.GetProperty(propertyInfo.Name).GetValue(self, null);
                        var toValue = type.GetProperty(propertyInfo.Name).GetValue(to, null);

                        if (selfValue != toValue && (selfValue == null || !selfValue.Equals(toValue)))
                        {
                            failedVal = propertyInfo.Name;
                            
                            return false;
                        }
                    }
                }
                failedVal = string.Empty;
                return true;
            }
            failedVal = string.Empty;
            return self == to;
        }
    }
