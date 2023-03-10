# Kogane Sorting Layer Editor Utility Internal

SortingLayerEditorUtility クラスの internal な機能にアクセスできる機能

## 使用例

```cs
using System;
using UnityEditor;
using UnityEngine;

namespace Kogane.Internal
{
    [CustomEditor( typeof( MeshRenderer ) )]
    internal sealed class MeshRendererInspector : Editor
    {
        private Editor             m_editor;
        private SerializedProperty m_sortingOrderProperty;
        private SerializedProperty m_sortingLayerIDProperty;

        public override void OnInspectorGUI()
        {
            if ( m_editor == null )
            {
                CreateCachedEditor
                (
                    targetObjects: targets,
                    editorType: Type.GetType( "UnityEditor.MeshRendererEditor, UnityEditor" ),
                    previousEditor: ref m_editor
                );
            }

            m_sortingOrderProperty   ??= serializedObject.FindProperty( "m_SortingOrder" );
            m_sortingLayerIDProperty ??= serializedObject.FindProperty( "m_SortingLayerID" );

            m_editor.OnInspectorGUI();
            serializedObject.Update();

            SortingLayerEditorUtilityInternal.RenderSortingLayerFields
            (
                sortingOrder: m_sortingOrderProperty,
                sortingLayer: m_sortingLayerIDProperty
            );

            serializedObject.ApplyModifiedProperties();
        }
    }
}
```

![ScreenShot00129](https://user-images.githubusercontent.com/6134875/212878813-c591e085-4fa3-497b-9449-2e0217c149ff.png)
